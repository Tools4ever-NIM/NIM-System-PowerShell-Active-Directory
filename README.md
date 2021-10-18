# NIM-Conn-System-PowerShell-Active-Directory
![image](https://user-images.githubusercontent.com/24281600/134387458-b0686b64-7252-41b0-9d6d-a8b084bac626.png)

This is a native connector. This repo is for additional tools specific to Active Directory

## Table of Contents
* [Getting Started](#getting-started)
* [Baseline Configuration](#baseline-configuration)
* [Automated Deletion](#automated-deletion)  

## Getting Started
* Create user account 
* Enable user account
* Disable user account
* Delete user account
* Group Membership (Add / Remove)

## Baseline Configuration
A baseline configuration is provided as a getting started example with all the recommended attributes, data types and relations.
See
[Config.Baseline.json](Config.Baseline.json)

## Automated Deletion

### Example Filter
![image](https://user-images.githubusercontent.com/24281600/134387022-fd8ba2b2-cc22-466d-b954-605d3cdd93c7.png)

![image](https://user-images.githubusercontent.com/24281600/134387051-0ea9975b-86eb-44ce-98b6-4e34b36e46a0.png)

### Populate Automated Deletion Date
This script column sets up the automated deletion date so it can be populated in AD when the account is disabled
```
let daysInFuture = 365;

let description = 'AUTOMATED - Delete After: ';
let date = new Date();
date.setDate(date.getDate() + daysInFuture);
let year = date.getUTCFullYear();
let month = date.getUTCMonth()+1;
let day = date.getUTCDate();

if (day < 10) { day = '0' + day; }
if (month < 10) { month = '0' + month; }

let deleteDate = '' + year + '-' + month + '-' + day

description += deleteDate;
return description;
```

### Evaluate Automated Deletion Date
This script column is used to determine if Automated Delete Date is in the future. If in the future, then the result is true.


```
let status = true;

let description = Users['description'];

if(description.includes('AUTOMATED - Delete After:'))
{
	description = description.replace('AUTOMATED - Delete After:','').trim();
  	try
    {
      let deleteDate = new Date(description);
      let todayDate = new Date();

      if(todayDate > deleteDate)
      {
       	status = false;
      }
      else
      {
      }
      
    }
    catch(e)
    {
    }
}

return status;
```


# NIM Docs
The official NIM documentation can be found at: https://docs.nimsuite.com
