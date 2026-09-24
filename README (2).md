# Linux Service and Process Supervisor

## Objective

This project is a beginner-friendly Operating Systems lab demonstration of how a Linux system monitors running processes and services, displays process information, and performs basic process-control operations.

## Problem Statement

Build a small Linux-based process and service supervisor that collects information about running processes, displays details such as PID, process name, state, CPU usage, and memory usage, and allows the user to perform basic control operations. The project should also provide service-status monitoring and handle invalid process IDs, unavailable services, and permission-related errors safely.

## Technologies Used

- C (C11 source with POSIX/Linux system calls)
- GCC
- GNU Make
- Linux, Ubuntu, or WSL

This project is intended for a Linux/Unix/POSIX environment. On Windows, compile and run the project inside WSL or another Linux virtual machine.

## System Calls, Linux Interfaces, and Functions Used

- `/proc` filesystem provides information about running processes and system resources.
- `kill()` sends signals to selected processes for supported control operations.
- `/proc/[PID]/status` provides process state and related information.
- `/proc/[PID]/stat` provides process statistics used for monitoring.
- `/proc/meminfo` provides system memory information.
- `systemctl` checks and manages Linux service status.
- `perror()` and return values report system and permission errors.
- Linux signals provide process-control mechanisms.

## Project Structure

```text
linux_service_process_supervisor/
├── process_supervisor.c
├── service_monitor.c
├── Makefile
└── README.md
```

## How the Programs Work

### `process_supervisor.c`

`process_supervisor` is the main process-monitoring component. It obtains information about running processes from the Linux `/proc` filesystem and presents useful details such as Process ID, process name, process state, CPU-related information, and memory usage.

The user can select a process using its PID and perform supported control operations. Linux signals are used to request actions such as stopping or terminating a selected process. The program checks the result of each operation and reports errors when a process does not exist or when permission is insufficient.

### `service_monitor.c`

`service_monitor` is the service-monitoring component. It checks the status of Linux services using the `systemctl` service-management interface and displays whether a selected service is active, inactive, failed, or unavailable.

The component helps demonstrate how operating-system services can be monitored from a system-programming application.

## Process Concepts

### Process Identification

Every running process in Linux is identified by a unique Process ID (PID). The supervisor uses the PID to locate and monitor a specific process.

### Process States

A Linux process can have different states during its lifetime, such as running, sleeping, stopped, or terminated. The supervisor reads process information from `/proc` to display the current state.

### Process Monitoring

The Linux `/proc` filesystem exposes information about active processes. The supervisor uses this information to display process details and resource usage.

### Signals and Process Control

Linux signals provide a mechanism for requesting actions from processes. The `kill()` system call is used to send a selected signal to a process. Appropriate error handling is used when the process cannot be accessed.

### Services

Linux services are background programs managed by the operating system. The project uses `systemctl` to inspect service status and demonstrate basic service supervision.

## Monitoring and Control Journey

```text
USER starts supervisor
          |
          v
   PROCESS SUPERVISOR
          |
          v
 Read /proc process data
          |
          v
 Display PID / Name / State
 CPU / Memory information
          |
          v
   USER SELECTS PID
          |
          v
  CONTROL OPERATION
   (Signal process)
          |
          v
   VERIFY PROCESS STATUS
          |
          v
   SERVICE MONITOR
          |
          v
 Check service using systemctl
          |
          v
      DISPLAY RESULT
```

This demonstrates process monitoring, process identification, resource observation, signals, process control, Linux services, system interfaces, and error handling.

## Compilation

From this directory, run:

```sh
make
```

The command creates the project executables defined by the Makefile.

To remove generated executables:

```sh
make clean
```

## Execution

Start the process supervisor with:

```sh
./process_supervisor
```

Start the service monitor with:

```sh
./service_monitor
```

Depending on the implementation and permissions, administrative privileges may be required for some process or service operations.

## Example Process Information

A typical process entry may be displayed in a format similar to:

```text
PID: 2456
Process: firefox
State: Running
CPU Usage: 2.4%
Memory Usage: 185 MB
```

The actual PID and resource values vary during every run.

## Example Service Status

A service-monitoring operation may display information similar to:

```text
Service: ssh
Status: Active
```

The service name and status depend on the Linux system being used.

## Testing Procedure

1. Build the project with `make` and confirm that the required executables are created.
2. Run `./process_supervisor`.
3. Verify that currently running processes are displayed.
4. Check that PID, process name, state, CPU usage, and memory information are shown.
5. Select a valid PID and verify that its information can be displayed.
6. Test a supported process-control operation on a suitable process.
7. Verify that invalid PIDs produce an appropriate error message.
8. Run `./service_monitor`.
9. Check the status of an available Linux service using the supervisor.
10. Test an unavailable or invalid service name and verify that the error is handled safely.
11. Verify that permission-related failures do not terminate the supervisor unexpectedly.

## Invalid Processes and Failure Handling

An invalid or non-existent PID should produce a clear error message without terminating the supervisor. If a process cannot be controlled because of insufficient permissions, the program should report the failure and continue safely.

Similarly, if a requested Linux service is unavailable or cannot be queried, the service monitor should report the problem clearly instead of crashing.

## Conclusion

The project demonstrates practical Linux process and service management by collecting process information, identifying processes using PIDs, monitoring process state and resource usage, sending signals for supported process-control operations, checking Linux service status, and handling common errors safely. It provides a simple practical demonstration of Operating Systems concepts including process management, process states, signals, resource monitoring, service management, and system-level interfaces.
