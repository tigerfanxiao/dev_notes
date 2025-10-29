# Questions:
### For Huawei 
1. for 170 employees in EU, what kind of resource is necessary? is there Identity as a Service as Microsoft Azure Active Directory in the Huawei cloud? 
2. How to implement remote device management on Huawei cloud. Say we have existing 170 thinkpad, do we need to deploy something client or configuration on each employee device? 
3. GDPR: According to recent Tiktok case, no China engineer should be allow to access the data in EU or even in Singapore
### For CTG
1. If we need to authenticate the user before them login windows? or do we need to deploy some remote device management service?
2. Domain or multiple-domain
3. Organization Unit
4. Group policy management, including email out send policy
5. User Object
	1. How to allow user to change their domain name password, do we need to deploy a self-service portal for user. Or for next phase?
	2. Default initial password
	3. Password expire policy
6. Devices: thinkpad
7. Any authentication with Windows License/Microsoft Office/Outlook?
8. OAuth 2.0 to other 3rd party cloud service or product as (Teams, zoom)
9. Any MFA requirement? Mobile authentication app?
10. DNS
11. Billing method: Monthly or Yearly
12. Maintenance team: Administrator (Spain, China HP, local IT team)

# Database Schema
### Group
- department_id - pk
- departmentname

### Users
- employee_id - pk
- name: firstname, lastname, middlename(nullable)
- email: belong to CTG domain, external resouce
- department: FK
- baselocation: country 
- Mobole number 
	- format: +country-prefix number
- role
- level 