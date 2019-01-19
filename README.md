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
===========================================
public Long getSequence() {
  org.springframework.jdbc.core.JdbcTemplate jdbcTemplateObject = new JdbcTemplate(dataSource);
  Long seq;
  String sql = "select SEQ_XY.NEXTVAL from dual";
  seq = jdbcTemplateObject.queryForObject(sql, new Object[] {}, Long.class);
  return seq;
}

=============================================================

package com.jcg;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.List;

/**
 * @author ashraf_sarhan
 *
 */
public class CsvFileReader {
	
	//Delimiter used in CSV file
	private static final String COMMA_DELIMITER = ",";
	
	//Student attributes index
	private static final int STUDENT_ID_IDX = 1;
	private static final int STUDENT_FNAME_IDX = 2;
	private static final int STUDENT_LNAME_IDX = 3;
	private static final int STUDENT_GENDER = 4; 
	private static final int STUDENT_AGE = 5;
	private static final int STUDENT_SALARY = 6;
	
	public static void readCsvFile(String fileName) {

		BufferedReader fileReader = null;
     
        try {
        	
        	//Create a new list of student to be filled by CSV file data 
        	List<Student> students = new ArrayList<Student>();
        	
            String line = "";
            
            //Create the file reader
            fileReader = new BufferedReader(new FileReader(fileName));
            
            //Read the CSV file header to skip it
            fileReader.readLine();
            
            //Read the file line by line starting from the second line
            while ((line = fileReader.readLine()) != null) {
                //Get all tokens available in line
                String[] tokens = line.split(COMMA_DELIMITER);
                if (tokens.length > 0) {
                	//Create a new student object and fill his  data
					Student student = new Student();
					
					student.setId(Long.parseLong(tokens[STUDENT_ID_IDX]));
					student.setFirstName(tokens[STUDENT_FNAME_IDX]);
					student.setLastName(tokens[STUDENT_LNAME_IDX]);
					student.setGender(tokens[STUDENT_GENDER]);
					student.setAge(Integer.parseInt(tokens[STUDENT_AGE]));
					student.setSalary(new BigDecimal(tokens[STUDENT_SALARY]));
					
					students.add(student);
				}
            }
            

            //Print the new student list
            for (Student student : students) {
				System.out.println(student.toString());
			}
        } 
        catch (Exception e) {
        	System.out.println("Error in CsvFileReader !!!");
            e.printStackTrace();
        } finally {
            try {
                fileReader.close();
            } catch (IOException e) {
            	System.out.println("Error while closing fileReader !!!");
                e.printStackTrace();
            }
        }

	}

}
