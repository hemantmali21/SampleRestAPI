# SampleRestAPI
Sample REST API
final String STATEMENT = "INSERT INTO User (id, col1) VALUES (ID.NEXTVAL, ?)";
final String GENERATED_COLUMNS[] = { "id" };
        
PreparedStatementCreator psc = new PreparedStatementCreator() {
    public PreparedStatement createPreparedStatement(Connection con) throws SQLException {
        PreparedStatement ps = con.prepareStatement(STATEMENT, GENERATED_COLUMNS);
        ps.setString(1, col1);
        return ps;
    }
};
        
GeneratedKeyHolder keyHolder = new GeneratedKeyHolder();
getJdbcTemplate().update(psc, keyHolder);
long id = keyHolder.getKey().longValue();
