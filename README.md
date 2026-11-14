# NSSM-Service-Manager

## Introduction

NSSM-Service-Manager is a Windows service management utility based on the Non-Sucking Service Manager (NSSM). It allows system administrators and IT professionals to install, configure, monitor, and manage applications as native Windows services. The software is particularly useful for running console applications, background processes, scripts, and server utilities that do not provide built-in Windows service functionality.

The primary purpose of NSSM is to bridge the gap between standard Windows applications and the Windows Service Control Manager (SCM). It enables programs to start automatically during system boot, run independently of interactive user sessions, and remain operational without requiring a user to stay logged in. The utility also monitors managed processes and can automatically restart applications when they terminate unexpectedly.

NSSM-Service-Manager provides a graphical interface for configuring service parameters, simplifying service installation and maintenance. Administrators can specify the executable file, command-line arguments, working directory, service name, and other application settings. Additional configuration options include environment variables, output redirection, process priority, CPU affinity, and application exit actions.

The utility supports both graphical and command-line administration, making it suitable for interactive configuration as well as automated deployment through scripts. It can also start, stop, restart, and query Windows services, including services not originally installed through NSSM.

Typical use cases include deploying Python applications, Node.js servers, Java processes, batch scripts, monitoring agents, and custom background workers as Windows services. By integrating these processes with Windows service management, NSSM-Service-Manager helps administrators standardize application startup, process supervision, and recovery procedures across Windows workstations and servers.

## How to Install and Configure Windows Services with NSSM

NSSM-Service-Manager allows administrators to register almost any executable application as a Windows service without modifying its source code. This is particularly useful for applications that normally run in a command prompt and lack native Windows service support.

To create a service, launch the NSSM graphical installation interface with administrator privileges and specify the application executable. The configuration includes several important parameters:

- **Application Path:** The full path to the executable file that NSSM will launch, such as `C:\Applications\Worker\worker.exe`.
- **Startup Directory:** The working directory used by the application. Specify it explicitly when the process depends on relative file paths or local configuration files.
- **Arguments:** Command-line parameters passed to the executable during startup.
- **Service Name:** A unique internal identifier used by Windows to register and manage the service.
- **Display Name:** The descriptive name displayed in the Windows Services console.
- **Description:** Additional information explaining the service's purpose.

For example, a Python background worker can be configured using `python.exe` as the application executable, the script directory as the startup directory, and `worker.py` as an application argument.

After configuring the required parameters, install the service and use the Windows Services console (`services.msc`) to verify its registration and startup configuration.

NSSM also supports command-line installation:

`nssm install DataWorker "C:\Python\python.exe" "C:\Applications\worker.py"`

The corresponding service can be managed with `nssm start DataWorker`, `nssm stop DataWorker`, and `nssm restart DataWorker`. To inspect its current state, execute `nssm status DataWorker`.

Correct executable paths, working directories, permissions, and startup dependencies are essential for reliable Windows service deployment. NSSM itself must remain at the installed path because Windows uses its executable as the service entry point.

## Windows Service Monitoring, Automatic Restart, and Logging Configuration

One of the main advantages of NSSM-Service-Manager is application process monitoring and automatic recovery. Unlike a conventional application launcher, NSSM supervises the process it starts and can respond when the managed application exits unexpectedly. This functionality is important for Windows servers, background workers, web applications, and other processes that must remain available.

Administrators can configure application exit actions to determine how NSSM handles different termination conditions. Depending on the configured policy, the manager can restart the application, ignore its exit, or stop the service. Exit-code-specific rules make it possible to distinguish successful termination from application failures.

For example, a continuously running Node.js server can be configured to restart after an unexpected crash while treating a normal exit code of `0` as successful termination. Restart throttling helps prevent excessive restart attempts when an application repeatedly fails during initialization.

Additional process management settings include:

- **I/O Redirection:** Redirect standard input, standard output, and standard error to files or other supported destinations. This makes console application output available for troubleshooting and operational monitoring.
- **Output File Rotation:** Configure log rotation based on file size and other available settings to prevent continuously growing log files from consuming excessive disk space.
- **Process Priority:** Assign an appropriate Windows process priority class to influence CPU scheduling.
- **CPU Affinity:** Restrict the managed process to selected logical processors when workload isolation is required.
- **Environment Variables:** Define application-specific variables required during service startup.
- **Windows Event Log:** Record NSSM operational events, including relevant application and service management errors.

For effective production deployment, administrators should combine application-level logging with NSSM process monitoring and Windows service recovery policies. They should also verify service account permissions, accessible log directories, restart behavior, and application shutdown handling. These measures help maintain reliable Windows background services and simplify troubleshooting when managed applications fail.
