# Windows Server 2019 Troubleshooting Guide

## Resolving: *“The user’s name being used for accessing the remote share folder is not recognized by the local computer”*

---

## 📄 Incident Information

| Item                  | Details                                                                                                  |
| --------------------- | -------------------------------------------------------------------------------------------------------- |
| **Date**              | October 18, 2024                                                                                         |
| **Issue Title**       | The user’s name being used for accessing the remote share folder is not recognized by the local computer |
| **Operating System**  | Windows Server 2019                                                                                      |
| **Server IP Address** | `192.168.1.10`                                                                                           |

---

# 📖 Overview

This issue typically occurs when a remote CIFS/SMB share attempts to authenticate using credentials that are not recognized by the local Windows Server.

The most common cause is:

* The remote SMB/CIFS username does not exist locally on the Windows Server
* Credential mismatches between the SMB client and the Windows Server
* Insufficient permissions assigned to the connecting user

---

# 🎯 Objective

Create a local Windows user account that:

* Matches the CIFS/SMB username and password
* Has appropriate backup and share access permissions
* Is added to the **Backup Operators** group

---

# 🧠 Brainstorming Troubleshooting Methods

## Possible Solutions

### A. Create a Matching Local User Account

Create a local user account on the Windows Server using:

* The same username
* The same password

as the remote CIFS/SMB authentication user.

---

### B. Assign Required Permissions

Add the newly created user to the following local group:

```text id="x4u2pz"
Backup Operators
```

This grants the user backup and restore privileges commonly required for SMB/CIFS operations.

---

# 🛠️ Working Troubleshooting Procedure

---

# Step 1 — Create a Local User Account

## Open Computer Management

1. Right-click the **Start** button.
2. Select:

```text id="k0i90r"
Computer Management
```

---

## Navigate to Local Users and Groups

1. In the left navigation pane, expand:

```text id="wzhy6d"
Local Users and Groups
```

2. Click:

```text id="hr3kzr"
Users
```

---

## Create a New User

1. Right-click inside the right pane.
2. Select:

```text id="8vt52m"
New User
```

3. Enter the following information:

| Field        | Action                                     |
| ------------ | ------------------------------------------ |
| **Username** | Use the same username as the CIFS/SMB user |
| **Password** | Use the same password as the CIFS/SMB user |

4. Fill in any additional fields if required.

5. Recommended configuration:

* Uncheck:

```text id="o4ztfh"
User must change password at next logon
```

6. Click:

```text id="mjlwm5"
Create
```

7. Click:

```text id="ozvkv8"
Close
```

---

# Step 2 — Add User to the Backup Operators Group

## Open Groups Section

1. In the **Computer Management** window, navigate to:

```text id="0hkx4s"
Local Users and Groups → Groups
```

---

## Open Backup Operators Group

1. Double-click:

```text id="7kkk9x"
Backup Operators
```

---

## Add the User to the Group

1. In the **Backup Operators Properties** window, click:

```text id="9ltbdq"
Add
```

2. Enter the username of the user you created earlier.

3. Click:

```text id="6ljn0q"
Check Names
```

to verify the account.

4. Once verified, click:

```text id="2z9on7"
OK
```

5. Click:

```text id="6md8s7"
OK
```

again to close the group properties window.

---

# ✅ Verification

To confirm the configuration:

1. Reopen the:

```text id="g4dg6z"
Backup Operators
```

group properties.

2. Verify the newly created user appears in the member list.

---

# 📌 Expected Result

After completing the above steps:

* The Windows Server should recognize the SMB/CIFS user
* Remote share authentication should succeed
* Backup-related access permissions should function correctly

---

# 🔍 Additional Notes

## Common Causes of This Error

| Cause                     | Description                                         |
| ------------------------- | --------------------------------------------------- |
| Username mismatch         | Local account does not match remote SMB credentials |
| Password mismatch         | Incorrect password used during authentication       |
| Missing permissions       | User lacks required group membership                |
| SMB authentication issues | NTLM/Kerberos negotiation failures                  |

---

# 🧪 Recommended Validation Tests

After configuration, test the following:

* Access the remote share manually
* Run backup jobs again
* Validate SMB authentication logs
* Check Event Viewer for authentication errors

---

# 📂 Administrative Tools Used

| Tool                   | Purpose                             |
| ---------------------- | ----------------------------------- |
| Computer Management    | Local user and group administration |
| Local Users and Groups | User/group management               |
| Backup Operators Group | Backup and restore permissions      |

---

# 🔒 Security Considerations

> ⚠️ Important

Accounts added to the **Backup Operators** group receive elevated privileges related to backup and restore operations.

Follow your organization’s security policies when assigning this permission.

---

# 🏁 Conclusion

Creating a matching local user account and assigning it to the **Backup Operators** group resolves authentication issues where the SMB/CIFS username is not recognized by the Windows Server.

This method is commonly used in:

* Backup integrations
* NAS authentication scenarios
* SMB share access troubleshooting
* Legacy application environments

---

# 📄 End of Document

**Prepared For:** IT Infrastructure & System Administration Teams

**Platform:** Windows Server 2019

**Document Type:** Troubleshooting Guide

**Last Updated:** October 18, 2024
