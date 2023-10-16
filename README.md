# UssefulMSSQL
**String Encryption**

CREATE FUNCTION [dbo].[ufn_EncryptString] ( @pClearString VARCHAR(100) )
RETURNS NVARCHAR(100) WITH ENCRYPTION AS
BEGIN
    
    DECLARE @vEncryptedString NVARCHAR(100)
    DECLARE @vIdx INT
    DECLARE @vBaseIncrement INT
    
    SET @vIdx = 1
    SET @vBaseIncrement = 128
    SET @vEncryptedString = ''
    
    WHILE @vIdx <= LEN(@pClearString)
    BEGIN
        SET @vEncryptedString = @vEncryptedString + 
                                NCHAR(ASCII(SUBSTRING(@pClearString, @vIdx, 1)) +
                                @vBaseIncrement + @vIdx - 1)
        SET @vIdx = @vIdx + 1
    END
    
    RETURN @vEncryptedString

END
GO

**String Decryption**
Now that the data is encrypted in the table, it is now time to provide the user-defined function that will decrypt the encrypted string.  Below is the user-defined function that performs the opposite of the encryption function above.

CREATE FUNCTION [dbo].[ufn_DecryptString] ( @pEncryptedString NVARCHAR(100) )
RETURNS VARCHAR(100) WITH ENCRYPTION AS
BEGIN

DECLARE @vClearString VARCHAR(100)
DECLARE @vIdx INT
DECLARE @vBaseIncrement INT

SET @vIdx = 1
SET @vBaseIncrement = 128
SET @vClearString = ''

WHILE @vIdx <= LEN(@pEncryptedString)
BEGIN
    SET @vClearString = @vClearString + 
                        CHAR(UNICODE(SUBSTRING(@pEncryptedString, @vIdx, 1)) - 
                        @vBaseIncrement - @vIdx + 1)
    SET @vIdx = @vIdx + 1
END

RETURN @vClearString

END
GO
