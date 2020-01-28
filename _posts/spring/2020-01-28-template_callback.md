---
title: Intellij Spring - 탬플릿/콜백 패턴
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-28 11:27:28 +0900'
categories: Spring
---

> ## 탬플릿/콜백 패턴


* ### UserDao.java
	```java
	public class UserDao {
			private DataSource dataSource;

			public UserDao() {
			}

			public void setDataSource(DataSource dataSource) {
					this.dataSource = dataSource;
			}

			public void add(User user) throws SQLException {
					Connection c = null;
					PreparedStatement ps = null;
					try {
							c = dataSource.getConnection();
							ps = c.prepareStatement("insert into user values(?, ?, ?)");
							ps.setString(1, user.getId());
							ps.setString(2, user.getName());
							ps.setString(3, user.getPassword());

							ps.executeUpdate();
					} catch (SQLException e) {
							throw e;
					} finally {
							if (ps != null) try {
									ps.close();
							} catch (SQLException e) {
							}
							if (c != null) try {
									c.close();
							} catch (SQLException e) {
							}
					}
			}

			public void deleteAll() throws SQLException {
					Connection c = null;
					PreparedStatement ps = null;
					try {
							c = dataSource.getConnection();
							ps = c.prepareStatement("truncate table user");
							ps.executeUpdate();
					} catch (SQLException e) {
							throw e;
					} finally {
							if (ps != null) try {
									ps.close();
							} catch (SQLException e) {
							}
							if (c != null) try {
									c.close();
							} catch (SQLException e) {
							}
					}
			}

			public User get(String id) throws SQLException {
					Connection c = null;
					PreparedStatement ps = null;
					ResultSet rs = null;
					User user = null;

					try {
							c = dataSource.getConnection();

							ps = c.prepareStatement("select * from user where id = ?");
							ps.setString(1, id);

							rs = ps.executeQuery();

							if (rs.next()) {
									user = new User(
													rs.getString("id"),
													rs.getString("name"),
													rs.getString("password")
									);
							}

							return user;
					} catch (SQLException e) {
							throw e;
					} finally {
							if (rs != null) try {
									rs.close();
							} catch (SQLException e) {
							}
							if (ps != null) try {
									ps.close();
							} catch (SQLException e) {
							}
							if (c != null) try {
									c.close();
							} catch (SQLException e) {
							}
							if (user == null) throw new EmptyResultDataAccessException(1);
					}
			}

			public int getCount() throws SQLException {
					Connection c = null;
					PreparedStatement ps = null;
					ResultSet rs = null;

					try {
							c = dataSource.getConnection();

							ps = c.prepareStatement("select count(id) from user");

							rs = ps.executeQuery();
							rs.next();
							return rs.getInt(1);
					} catch (SQLException e) {
							throw e;
					} finally {
							if (rs != null) try {
									rs.close();
							} catch (SQLException e) {
							}
							if (ps != null) try {
									ps.close();
							} catch (SQLException e) {
							}
							if (c != null) try {
									c.close();
							} catch (SQLException e) {
							}
					}
			}
	}	
	```
* ### 기본적인 탬플릿/콜백 패턴
	* #####  StatementStrategy.java
		```java
		public interface StatementStrategy {
				PreparedStatement makePreParedStatement(Connection c) throws SQLException;
		}		
		```
	* ##### JdbcContext.java
		```java
		public class JdbcContext {
				private DataSource dataSource;

				public void setDataSource(DataSource dataSource) {
						this.dataSource = dataSource;
				}

				public void updateJdbcContextWithStatementStrategy(StatementStrategy stmt) throws SQLException {
						Connection c = null;
						PreparedStatement ps = null;

						try {
								c = dataSource.getConnection();
								ps = stmt.makePreParedStatement(c);
								ps.executeUpdate();
						} catch (SQLException e) {
								throw e;
						} finally {
								if (ps != null) try {
										ps.close();
								} catch (SQLException e) {
								}
								if (c != null) try {
										c.close();
								} catch (SQLException e) {
								}
						}
				}

				public void executeSql(final String query, final String... params) throws SQLException {
						updateJdbcContextWithStatementStrategy(new StatementStrategy() {
								public PreparedStatement makePreParedStatement(Connection c) throws SQLException {
										PreparedStatement ps = c.prepareStatement(query);
										for (int i = 1; i < params.length + 1; i++) {
												ps.setString(i, params[i - 1]);
										}
										return ps;
								}
						});
				}
		}		
		```
	* ##### UserDao.java
		```java
		public class UserDao {
				private DataSource dataSource;
				private JdbcContext jdbcContext;

				public void setDataSource(DataSource dataSource) {
						this.jdbcContext = new JdbcContext();
						this.jdbcContext.setDataSource(dataSource);

						this.dataSource = dataSource;
				}

				public void add(final User user) throws SQLException {
						jdbcContext.executeSql("insert into user values(?, ?, ?)",
										user.getId(),
										user.getName(),
										user.getPassword());
				}

				public void deleteAll() throws SQLException {
						jdbcContext.executeSql("truncate table user");
				}
			...
		}
		```
##### executeUpdate() 에서 반복되는 try/catch/finally 부분을 기본적인 탬플릿/콜백 패턴을 사용해 중복을 제거했다.
##### 위의 코드는 일종의 전략 패턴이 적용된것 이라고 볼 수 있다. 복잡하지만 바뀌지 않는 일정한 패턴을 갖는 작업 흐름이 존재하고 그중 일부분만 자주 바꿔서 사용해야 하는 경우에 적합한 구조다. 전략 패턴의 기본 구조에 익명 내부 클래스를 활용한 방식이다. 이런 방식을 스프링에서는 탬플릿/콜백 패턴이라고 부른다.
##### 전략패턴의 컨텍스트를 탬플릿이라 부르고,익명 내부 클래스로 만들어지는 오브젝트를 콜백이라고 부른다.


* ### 스프링의 JdbcTemplate
	* ##### UserDao.java
		```java
		public class UserDao {
				public void setDataSource(DataSource dataSource) {
						this.jdbcTemplate = new JdbcTemplate(dataSource);
				}

				private JdbcTemplate jdbcTemplate;

				private RowMapper<User> userMapper =
								new RowMapper<User>() {
										public User mapRow(ResultSet resultSet, int i) throws SQLException {
												return new User(resultSet.getString("id"),
																resultSet.getString("name"),
																resultSet.getString("password"));
										}
								};


				public void add(final User user) {
						jdbcTemplate.update("insert into user values(?, ?, ?)",
										user.getId(),
										user.getName(),
										user.getPassword());
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

				public int getCount() {
						return this.jdbcTemplate.queryForObject("select count(id) from user", Integer.class);
				}
		}
		```
##### Spring에서 제공하는 JdbcTemplate을 사용해 탬플릿/콜백 패턴을 구현.

> ###### [토비의 스프링 3.1]


[토비의 스프링 3.1]: https://book.naver.com/bookdb/book_detail.nhn?bid=7006516