USE [AdventureWorks2017]
GO

DECLARE @EnderecoID INT = 1;
DECLARE @NovoAddressLine1 NVARCHAR(60) = 'Novo ';
DECLARE @existe INT;

-- Verificar se o registro existe
SELECT @existe = COUNT(1) 
FROM [AdventureWorks2017].[Person].[Address] 
WHERE AddressID = @EnderecoID;

-- Inserir apenas se o registro existir
IF @existe > 0
BEGIN
    -- Ativar IDENTITY_INSERT
    SET IDENTITY_INSERT A_Tabela_destino ON;
    
    -- Inserir dados com AddressLine1 personalizado
    INSERT INTO A_Tabela_destino (AddressID, AddressLine1, AddressLine2, City, StateProvinceID, PostalCode, SpatialLocation, rowguid, ModifiedDate)
    SELECT 
        AddressID, 
        @NovoAddressLine1,
        '1', 
        City, 
        StateProvinceID, 
        PostalCode, 
        SpatialLocation, 
        rowguid, 
        ModifiedDate
    FROM [AdventureWorks2017].[Person].[Address]
    WHERE AddressID = @EnderecoID;
    
    -- Desativar IDENTITY_INSERT
    SET IDENTITY_INSERT A_Tabela_destino OFF;
    
    PRINT 'Registro inserido com sucesso.';
END
ELSE
BEGIN
    PRINT 'Registro com AddressID = ' + CAST(@EnderecoID AS VARCHAR(10)) + ' não encontrado.';
END
