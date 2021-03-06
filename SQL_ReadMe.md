# Stored Procedures and Functions in SQL
# Stored Procedures
## What is store procedures?
  + Pre-compiled objects that is saved to execute anywhere.
  + Allow us to store repeated queries for reuse.
    ## Advantages
      ### Performance
          + Compiled once stored in executable form.
            + Allows for quick and efficient procedure calls
          + Exe. is automatically cached 
            + Lowers memory requirements and increases application speed
      ### Productivity and ease
          + Logic can be handle in store procedures
          + Called by programmatic interfaces 
      ### Sevurity controls
          + Can grant user permission to execute a store procedure, independently
    ## Example
        CREATE PROCEDURE myProcedure AS PRINT 'This is my store procedure:)'
    ## Execute store procedure
        exec myProcedure


# Functions
  ## What is function?
  + Functions allow us to create functions to perform complex calculations
  + Return result as a value

    ### Built-in functions
        + Cannot be modified
        + Can be referenced only in  Transact-SQL statements
    ### User-defined functions
        + Use *CREATE FUNCTION* to generate a function
        + Can take 0+ parameters
        + returns a single data value
  ## Advantages
  + Advantages of functions

    ### Modularity
        + Can create a function once that is stored in your database
        + You can use the function anywhere
      ### Execution
        + Reduces compilation cost **Transact-SQL** by caching the statments and reuse them.

  ## Examples
  + Function examples
    ### Returning a Table
        CREATE FUNCTION fxn_example (
          @varName Type
        )
        RETURN TABLE AS
        RETURN
              SELECT * FROM Table_name WHERE table_name.varName >= @varName;
        
        **Usage**
        SELECT * FROM database_object.fxn_example(20)

    ### Returns a SCALAR
        CREATE FUNCTION number_people_over_equal(
          @age INT
        )
        RETURNS INT AS
        BEGIN
          DECLARE @returnvalue INT;

          SELECT @returnvalue = COUNT(*) 
            FROM PERSONS
            WHERE Age >= @age;
          
          RETURN(@returnvalue)
        END;




# Summary
    | Difference        |      Store Procedure(SP)          |  Functions (Fxn)           |
    |-------------------|:---------------------------------:|---------------------------:|
    | Return type       | Optional. Can even return 0       | Must return a value type   |
    | Calls             | Cannot be called from Fxn         | Can be called in SP        |
    | Statements usage  | INSERT, UPDATE, DELETE, SELECT    | Only SELECT statment       |





####################################################################################

# Error handling
  + Control over Transact - SQL code
  + Translate message into more readible and understandable text
    + Makes logging easy to track

    ## Two types
      ### System-defined exceptions
        + Predefined exceptions 
          + ArithmeticException
         

      ### User-defined exceptions
        + When you call **Throw**
      ### Example
        THROW [ { error_number | @local_variable },  
        { message | @local_variable },  
        { state | @local_variable } ]

        + error_number - Is a constant or variable that represents the exception. error_number is int and must be greater than or equal to 50000 and less than or equal to 2147483647.

        + message - Is a string or variable that describes the exception. message is nvarchar(2048).

        + state - Is a constant or variable between 0 and 255 that indicates the state to associate with the message. state is tinyint.



    ## Structure
        BEGIN TRY  
        --code to try 
        END TRY  
        BEGIN CATCH  
            --code to run if an error occurs
        --is generated in try
        END CATCH

    ## Key terms
        + *ERROR_NUMBER* ??? Returns the internal number of the error
        + *ERROR_STATE* ??? Returns the information about the source
        + *ERROR_SEVERITY* ??? Returns the information about anything from informational errors to errors user of DBA can fix, etc.
        + *ERROR_LINE* ??? Returns the line number at which an error happened on
        + *ERROR_PROCEDURE* ??? Returns the name of the stored procedure or function
        + *ERROR_MESSAGE* ??? Returns the most essential information and that is the message text of the error


    ## Another example of error handling
        BEGIN TRY
        -- Generate a divide-by-zero error  
          SELECT
            1 / 0 AS Error;
        END TRY
        BEGIN CATCH
          SELECT
            ERROR_NUMBER() AS ErrorNumber,
            ERROR_STATE() AS ErrorState,
            ERROR_SEVERITY() AS ErrorSeverity,
            ERROR_PROCEDURE() AS ErrorProcedure,
            ERROR_LINE() AS ErrorLine,
            ERROR_MESSAGE() AS ErrorMessage;
        END CATCH;
