learned more about partitioning with parted and having to format with mkfs.ext4 etc (different command for each type)
and probably learned more before.
didnt log them so...



learned about PAM (Pluggable Authentication Modules)
does password things.

learned it while configuring ly (a display manager/login manager)

PAM is a Linux library (libpam, part of the Linux-PAM project. deeply integrated into the system) which applications or services delegates the authentication process to by calling functions provided in the PAM library, which will execute *security modules based on the configurations in /etc/pam.d/.
*security modules: think as actions like "check if the password is 8 characters or more"
This means PAM provides a centralized (not spread out across the filesystem) way to control & audit every authentication process.
other than that, the existence of PAM means that applications can not arbitrarily implement their own authentication process and risk security. by having PAM, a dedicated authentication library, authentication across different services are as consistent as desired.

Location: Library is located in /lib/x86-64-linux-gnu/libpam* (spread across files). Configs are located in /etc/pam.d.
As for the security modules, they are located in /lib/x86_64-linux-gnu/security/, and module-specific confs are in /etc/security.

Four Core Functions that PAM handles:
1. Authentication (credential validation)
2. Password (handles password changes, e.g. password quality checks)
3. Account (checks account status, permissions, restrictions)
4. Session (manages user environment, e.g. what happens post login)

in /etc/pam.d is contained configuration files for certain services. in these configs, we can define *stacks for that certain service.

*Stack: Sequence of security modules executed (Module execution is done from top to bottom)
Once the stack is finished, it returns a success or a failure.

Config parts
1st field: Defines which aspect of the core functions it is implementing
auth, password, account, session

2nd field: Control flag. Defines behavior of the authentication upon a module succeeding or failing (e.g. passing a check or not)
required: Failure → the stack fails, but all next modules run
requisite: Failure → the stack immediately fail, stop processing next modules in that stack.
sufficient: Success → immediate success (skip rest), unless a previous 'required' failed
optional: Whether the module succeeds or fails doesn't affect the final status or execution of the rest of the stack

3rd field: the module to execute (analogy: this is a function that receives the password as parameter, outputs based on parameter) that returns a success/fail
These modules are:
-pwquality (checks password quality)
-pwhistory (looks at the history and uh... just go read manual)
-unix (standard auth)
-faillock (lock out an account)
-faildelay (fail upon certain time passed)
and more
+ Important modules according to deepseek:
    pam_sss.so (System Security Services Daemon - for AD/LDAP)

    pam_krb5.so (Kerberos)

    pam_ldap.so (LDAP directly)

    pam_rootok.so (Allow root without password)

    pam_wheel.so (Restrict to wheel group)

    pam_limits.so (Set ulimits - VERY important!)


4th field: module arguments
Arguments are just like flags (such as find *-type f -name .bashrc* < these are flags). They customize how each module behaves.
you can put multiple arguments in one line.
Arguments are specific for each module, so you have to search up each argument 

Config file Syntax!:
type    control_flag    module.so    argument1=value1 argument2 argument3=value

there are recommended configurations that are defined in DISA STIG and CIS Benchmarks

+ you can @include other_config_file_name/directory to execute the stack from that other config file.

PAM is a Linux/Unix authentication method, that is not very universal, but still exists in a lot of different systems.
PAM primarily sits inside linux systems as a core library, but exists in other systems such as macOS and Solaris (in fact, PAM first appeared in Solaris). Some systems use concepts similar to PAM. Windows and Android use completely different frameworks.
PAM isn't implemented in systems (usually imbedded systems, or containers like in docker) that dont require authentication.
