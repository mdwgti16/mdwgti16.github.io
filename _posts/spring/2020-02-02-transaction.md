---
title: Intellij Spring - 트랜잭션
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-2-02 17:27:28 +0900'
categories: Spring
---

> ## 트랜잭션


* ### JDBC를 이용한 트랜잭션
	```java
	Connection c = dataSource.getConnection();

	c.setAutoCommit(false);
	try{
			PreparedStatement ps1 = c.prepareStatement("update user set ...");
			ps1.executeUpdate();

			PreparedStatement ps2 = c.prepareStatement("update user set ...");
			ps2.executeUpdate();

			c.commit();
	}catch (SQLException e){
			c.rollback();
	}

	c.close();
	```
##### JDBC에서 트랜잭션을 시작하려면 자동커밋 옵션을 false로 만들어주고 정상적으로 모든 동작이 실행되면 commit(), 그렇지 않으면 rollback()으로 트랜젝션을 종료하면 된다. 트랜잭션의 경계는 하나의 Connection이 만들어지고 닫히는 범위 안에 존재한다. 
##### 스프링의 JdbcTemplate을 사용하게 되면 직접적으로 Connection을 사용하지 않는다. JdbcTemplate의 메소드 내에서 트랜젝션이 새로 만들어지고 메소드가 빠져나오기 전에 종료되기 때문에 위와 같은 방법으로는 JdbcTemplate에서 트랜젝션의 경계설정이 불가능하다.
* ### 트랜잭션 동기화
	```java
	public class UserDaoJdbc implements UserDao {
			public void setDataSource(DataSource dataSource) {
					this.jdbcTemplate = new JdbcTemplate(dataSource);
			}

			private JdbcTemplate jdbcTemplate;

			private RowMapper<User> userMapper =
							new RowMapper<User>() {
									public User mapRow(ResultSet resultSet, int i) throws SQLException {
											return new User(resultSet.getString("id"),
															resultSet.getString("name"),
															resultSet.getString("password"),
											Level.valueOf(resultSet.getInt("level")),
											resultSet.getInt("login"),
											resultSet.getInt("recommend"),
															resultSet.getString("email"));
									}
							};


			public void add(final User user) {
					jdbcTemplate.update("insert into user values(?, ?, ?, ?, ?, ?,?)",
									user.getId(),
									user.getName(),
									user.getPassword(),
									user.getLevel().intValue(),
									user.getLogin(),
									user.getRecommend(),
									user.getEmail());
			}

			public void deleteAll() {
					this.jdbcTemplate.update("truncate table user");
			}

			public User get(String id) {
					return this.jdbcTemplate.queryForObject("select * from user where id = ?",
									new Object[]{id},
									this.userMapper);
			}

			public List<User> getAll() {
					return this.jdbcTemplate.query("select * from user",
									this.userMapper);

			}


			public void update(User user) {
					jdbcTemplate.update("update user set name = ?, password = ?, level = ?, login = ?, recommend = ? where id = ?",
									user.getName(),
									user.getPassword(),
									user.getLevel().intValue(),
									user.getLogin(),
									user.getRecommend(),
									user.getId());
			}

			public int getCount() {
					return this.jdbcTemplate.queryForObject("select count(id) from user", Integer.class);
			}
	}	
	```
	```java
	public void upgradeLevels() throws SQLException {
			TransactionSynchronizationManager.initSynchronization();
			Connection c = DataSourceUtils.getConnection(dataSource);
			c.setAutoCommit(false);

			try {
					List<User> users = userDao.getAll();
					for (User user : users) {
							if (canUpgradeLevel(user))
									upgradeLevel(user);
					}
					c.commit();
			}catch (Exception e) {
					c.rollback();
					throw e;
			}finally {
					DataSourceUtils.releaseConnection(c,this.dataSource);
					TransactionSynchronizationManager.unbindResource(this.dataSource);
					TransactionSynchronizationManager.clearSynchronization();
			}
	}
	```
##### TransactionSynchronizationManager는 스프링에서 제공하는 트랜잭션 동기화 관련 클래스이다. 이 클래스를 이용해 먼저 트랜잭션 동기화 작업을 초기화하도록 한다.
##### DataSourceUtils의 getConnection() 메소드는 Connection 오브젝트 생성과 트랜잭션 동기화를 위한 저장소 바인딩을 해주기 때문에 DataSource가 아니라 DataSourceUtils을 사용한다.


* ### 트랜잭션 서비스 추상화
##### 위의 upgradeLevels() 메소드는 하나의 DB로 JDBC를 사용하는 로컬 트랜잭션 처리 방식이다. 하지만 DB를 여러개 사용하게 될 경우 하나의 트랜잭션 안에서 여러 개의DB에 데이터를 넣는 작업을 해야 할 필요가 있다. 이때는 글로벌 트랜잭션 방시을 사용해야 한다. 하나 이상의 DB가 참여하는 트랜잭션을 만들기 위해서는 JTA(Java Transaction Api)를 사용해야 한다. 
##### 문제는 UserService의 로직이 바뀌지 않아도 로컬 트랜잭션이면 충분한  JDBC를 이용한 트랜잭션 관리 코드에서는 위와 같은 코드로, 다중 DB를 사용하는 글로벌 트랜잭션 환경에서는 JTA를 사용하는 코드로, 더 나아가 하이버네이트를 사용하게되면 또 그에 맞춰 하이버네이트의 Session과 Transaction 오브젝트를 사용하는 코드로 매번 코드를 변경을 해야된다는 점이다. 즉 UserService가 특정 트랜잭션 방법에 의존하는 코드가 되버린다.
##### 다행이도 스프링은 트랜잭션 기술의 공통점을 담은 트랜잭션 추상화 기술을 제공하고 있다. 이를 이용하면 애플리케이션에서 직접 각 기술의 트랜잭션 API를 이용하지 않고도, 일관된 방식으로 트랜잭션을 제어하는 트랜잭션 경계설정 작업이 가능해진다.
	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
				 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
				 xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
			...

			<bean id="userService" class="ex.spring.user.service.UserService">
					...
					<property name="transactionManager" ref="transactionManager"/>
			</bean>

			<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
					<property name="dataSource" ref="dataSource"/>
			</bean>
			
			...
	</beans>	
	```
	```java
	public class UserService {
			...
			private PlatformTransactionManager transactionManager;

			public void setTransactionManager(PlatformTransactionManager transactionManager) {
					this.transactionManager = transactionManager;
			}

			public void upgradeLevels() {
					TransactionStatus status = this.transactionManager.getTransaction(new DefaultTransactionDefinition());

					try {
							List<User> users = userDao.getAll();
							for (User user : users) {
									if (canUpgradeLevel(user))
											upgradeLevel(user);
							}
							this.transactionManager.commit(status);
					}catch (RuntimeException e) {
							this.transactionManager.rollback(status);
							throw e;
					}
			}
			...
	}	
	```
##### 스프링이 제공하는 모든 PlatformTransactionManager을의 구현 클래스는 싱글톤으로 사용이 가능하다.
##### 따라서 PlatformTransactionManager을 빈으로 등록해 스프링 DI의 방식을 이용하면 특정 트랜잭션에 의존적이지 않은 UserService 클래스가 가능해진다.

> ###### [토비의 스프링 3.1]


[토비의 스프링 3.1]: https://book.naver.com/bookdb/book_detail.nhn?bid=7006516