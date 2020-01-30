![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Use SQL To Query Active Directory Quicky With ADSI Linked Server
**Post Date: March 19, 2018**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>Whenever you want to make sure your SQL Security is up to scratch it's a good idea to query Active Directory and cross reference accounts or groups with those added to SQL Server. One way to do this is by creating a linked server to Active Directory and running a simple select query.
Below is a some SQL Logic to get you started. Remember you can query OUs, Groups, and Users directly if needed.</p>      



## SQL-Logic
```SQL
use master;
set nocount on
 
-- Create ADSI Linked Server for Active Directory queries
exec master.dbo.sp_addlinkedserver 
    @server     = N'ADSI_LINK'
,   @srvproduct = N'Active Directory Services Interfaces'
,   @provider   = N'ADSDSOObject'
,   @datasrc    = N'MyDomainController.domain.com'
go
exec master.dbo.sp_addlinkedsrvlogin 
    @rmtsrvname = N'ADSI_LINK'
,   @useself    = N'False'
,   @locallogin = NULL
,   @rmtuser    = N'MyDomain\MyUserName'
,   @rmtpassword    = 'MyPassword'
go
 
select * from openquery
(
    ADSI_LINK
,   'SELECT mail
    ,   displayname FROM ''LDAP://MyDomainController.MyDomain.Com''
    WHERE 
        objectCategory  = ''Person'' 
        AND objectClass = ''user''
')
```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

     
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

