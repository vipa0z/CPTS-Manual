	## 🟢 Quick & Easy Checks (Low Effort, High Reward)
- [ ] Obtain [**System information**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#system-info)
- [ ] Check [**current** user **privileges**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#users-and-groups)
- [ ] Are you [**member of any privileged group**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#privileged-groups)?/domain groups?
- [ ] Check if you have [sensitive tokens](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#token-manipulation) enabled (**SeImpersonatePrivilege, SeDebugPrivilege, etc.**)
- [ ] [**AlwaysInstallElevated**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#alwaysinstallelevated)?
- [ ] Interesting info in [**env vars**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#environment)?
- [ ] Passwords in [**PowerShell history**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#powershell-history)?
- [ ] Check [**users homes**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#home-folders)
- [ ] [**Password Policy**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#password-policy)
- [ ] What’s in the [**Clipboard**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#get-the-content-of-the-clipboard)?
- [ ] [**User Sessions**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#logged-users-sessions)
- [ ] [**Cached Credentials**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#cached-credentials)?
- [ ] [**UAC bypasses**](https://github.com/carlospolop/hacktricks/blob/master/windows-hardening/authentication-credentials-uac-and-efs/uac-user-account-control/README.md)

---

## 🟡 Medium Effort (Enumeration & Targeted Exploits)
### Services & Processes
- [ ] [Can you **modify any service**?](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#permissions)
- [ ] [Unquoted service paths](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#unquoted-service-paths)?
- [ ] [Service registry modifications](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#services-registry-modify-permissions)?
- [ ] Processes binaries [**file and folders permissions**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#file-and-folder-permissions)
- [ ] Steal credentials from **interesting processes** (`ProcDump.exe` on browsers, etc.)
- [ ] [**Insecure GUI apps**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#insecure-gui-apps)
- [ ] [**Memory Password mining**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#memory-password-mining)

### Applications / DLL Hijacking
- [ ] **Write perms on installed apps**?
- [ ] [**Startup Applications** abuse](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#run-at-startup)?
- [ ] [**DLL Hijacking**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#path-dll-hijacking): PATH folders writable? Non-existent DLLs?
- [ ] [**Vulnerable Drivers**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#drivers)

### Network
- [ ] Check **current** [**network info**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#network)
- [ ] Internal ports running local services `netstat`
- [ ] Enumerate shares, routes, neighbors
- [ ] Look at hidden services restricted externally

## 🔴 Harder / Advanced Paths (Last Resort)
- [ ] [**WSUS exploit**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#wsus)?
- [ ] [**Winlogon credentials**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#winlogon-credentials)
- [ ] [**Windows Vault**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#credentials-manager-windows-vault)
- [ ] [**DPAPI credentials**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#dpapi)?
- [ ] Saved [**RDP connections**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#saved-rdp-connections)?
- [ ] [**Wifi passwords**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#wifi)
- [ ] Credentials in [**recent commands**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#recently-run-commands)?
- [ ] [**AppCmd.exe / SCClient.exe** abuse](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#scclient-sccm)
- [ ] [**Files & Registry Credentials**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#files-and-registry-credentials) (Putty, unattended.xml, SAM, IIS configs, browser creds, cloud creds, etc.)
- [ ] [**Leaked Handlers**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#leaked-handlers)?
- [ ] [**Pipe Client Impersonation**](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#named-pipe-client-impersonation)

---

## 🕳️ Last Resort
  - [ ] Use **scripts** (WinPEAS, jaws, seatbelt, Watson, Sherlock, etc.)
  - [ ] Use **searchsploit**
  - [ ] Use **Google**