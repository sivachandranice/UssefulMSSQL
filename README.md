# UssefulMSSQL
CREATE OR ALTER FUNCTION [dbo].[SuSy_EncryptString] ( @pClearString VARCHAR(100) )
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


CREATE OR ALTER FUNCTION [dbo].[SuSy_DecryptString] ( @pEncryptedString NVARCHAR(100) )
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
