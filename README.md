# **Access-Based Enumeration**

.

**What is Access-Based Enumeration (ABE)?**

ABE enumeration is a feature in windows server that hides files and folders from users who do not have permission to access them. Users are only see what they are allowed to open. It’s available on Windows Server for shared folders. 

---

**Why is ABE important?**

First, it enhances security. ABE prevents users from even knowing certain folders exist if they don’t have the required permissions. This reduces the risk of unauthorized access attempts and helps protect sensitive information from prying eyes.

Next, It reduces user confusion. Without ABE, users Might see many folders they can’t access. This can be frustrating and confusing especially in environments with large shared drives. ABE presents a cleaner, more intuitive experience by only showing relevant folders.

Next, It also protects privacy of sensitive folders departments like HR and Finance may store confidential data. So, AB ensures that folders like HR are invisible to non-HR users even if they know the path. This helps enforce privacy policies and data protection regulations. 

Another reason is it limits unnecessary access attempts. Users often click on folders they see out of curiosity even if they don’t have access. 

ABE prevents these clicks entirely by removing temptation. This reduces the number of failed access attempts, logs, and potential alerts and it also reduces help desk tickets. When users sees folders they can’t open they might assume something is broken and submit support tickets. With ABE enabled they only see what they have permission to use, leading to a fewer why can’t I access this folder queries to IT.

---

**How ABE Works?**

AB relies on NTFS and share permissions. It filters the file or folder view based on user access. So if a user has no permission, the folder is hidden. 

---

**Important Notes on ABE.**

ABE doesn’t change permissions-just visibility. AB only hides files and folders from users who don’t have permission to access them. It doesn’t grant them or deny access. it simply prevents users from seeing what they already don’t have access to.

**AB works only on NTFS Volumes.** AB is a feature deeply tied into NTFS permission structure, so it can’t function on older or simpler file systems. This really important to note that AB is only supported on volumes that are formatted NTFS and it doesn’t work with FAT 32, XFAT, or, other file systems. Next AB doesn’t protect files from being accessed if permissions are misconfigured. AB just hides and it doesn’t secure. Always double check your permissions. ABE will not save you from misconfigurations. ABE relies on proper NTFS permissions. If you accidently give a user access to a sensitive file, AB won’t stop them. It will show the file because the system thinks they are authorized.

---

Perquisites: 

- Windows sever 2016,2019, or 22
- File Server Role installed
- At least one windows client joined to the same domain
- A domain enviroment
- Create test users and groups in Active Directory
- A Shared folder created on your server

---

Real world Scenario: 

You’re simulating a file server for the HR and IT departments. You want users from HR to only see HR-related folders and IT to only see theirs even if all of the folders are inside the same shared directory.

**Create users and groups** 

Step 1: Open Active directory  

Step 2: Under Users → Under HR→ create a new group → name it “HR_depart”.

Step 3: Under Users → Under IT→ create a new group → name it “IT-depart”.

Step 4: Under IT → create a new user → name it Lacus Clyne → Select password never expires → Click OK

Step 5: Add “Lacus Clyne” to IT Department → Right Click → Click Properties → Select Members Tab → Select Add → Click OK

Step 6:  Under HR → create a new user → name it Conan Doyle → Select password never expires → Click OK

Step 7: Add “Conan Doyle” to HR Department → Right Click → Click Properties → Select Members Of Tab → Select Add → Click OK

---

**Create a Shared Folder with Subfolder and NTFS’s Permissions**

Step 1: Open File Explorer → Open Local (C:) Drive

Step 2: Right click → Create a new folder → Name it “Department Shares”

Step 3: Create Two New folders Name them HR and IT.

Step 4: Right click HR Folder → Security Tab → Click Advanced → Enabled Inheritance → click on convert inheritance Permissions → Remove all Users 

Step 5: Click Edit→ Add HR Group → Click Ok → Allow Full Control 

Step 6: Right click IT Folder → Security Tab → Click Advanced → Enabled Inheritance → click on convert inheritance Permissions → Remove all Users 

Step 7: Click Edit→ Add IT Group → Click Ok → Allow Full Control 

**Share The Parent Folder.**

Step 8: Right click “Dept Shares” → Click Sharing Tab → Click Advance Sharing → Click Share this folder → Go to permissions → Remove Everyone → Add “HR-Dept” and “IT Dept” → Only Allow Read → Click OK

**Enable Access-Based Enumeration.**

Step 9: Open File Server Manager 

Step 10: On the left side click File Storage Services → Click Shares → Right Click “Deptshares” → Select Properties → Select settings (left side) → Enable Access-Based Enumeration → Click OK and Apply

**Testing The File Permissions.**

Step 11: Verify it works → Go to Client Machine → Log into “Conan Doyle” and He should have access to HR folder and not see the IT folder.

Step 12: Open File Explorer → Enter Windows Host Name → Should see shared folders in Network → Click Dept Share → Should only see HR

Step 13: Make sure it is not a member of Active Directory Domain Services Folder

Step 14: Verify it works → Go to Client Machine → Log into “Lacus Clyne” 

Step 15:  Open File Explorer →  Should see shared folders in Network → Click Dept Share → Should only see IT
