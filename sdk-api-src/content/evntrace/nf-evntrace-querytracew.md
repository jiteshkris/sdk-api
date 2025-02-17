---
UID: NF:evntrace.QueryTraceW
title: QueryTraceW function (evntrace.h)
description:
  The QueryTrace function retrieves the property settings and session statistics
  for the specified event tracing session. The ControlTrace function supersedes
  this function.
helpviewer_keywords:
  [
    "QueryTrace",
    "QueryTrace function [ETW]",
    "QueryTraceA",
    "QueryTraceW",
    "_evt_querytrace",
    "base.querytrace",
    "etw.querytrace",
    "evntrace/QueryTrace",
    "evntrace/QueryTraceA",
    "evntrace/QueryTraceW",
  ]
old-location: etw\querytrace.htm
tech.root: ETW
ms.assetid: 8ad0f4f6-902c-490e-b26e-7499dd99fc95
ms.date: 12/05/2018
ms.keywords:
  QueryTrace, QueryTrace function [ETW], QueryTraceA, QueryTraceW,
  _evt_querytrace, base.querytrace, etw.querytrace, evntrace/QueryTrace,
  evntrace/QueryTraceA, evntrace/QueryTraceW
req.header: evntrace.h
req.include-header:
req.target-type: Windows
req.target-min-winverclnt: Windows 2000 Professional [desktop apps \| UWP apps]
req.target-min-winversvr: Windows 2000 Server [desktop apps \| UWP apps]
req.kmdf-ver:
req.umdf-ver:
req.ddi-compliance:
req.unicode-ansi: QueryTraceW (Unicode) and QueryTraceA (ANSI)
req.idl:
req.max-support:
req.namespace:
req.assembly:
req.type-library:
req.lib: Advapi32.lib
req.dll: Advapi32.dll
req.irql:
targetos: Windows
req.typenames:
req.redist:
ms.custom: 19H1
f1_keywords:
  - QueryTraceW
  - evntrace/QueryTraceW
dev_langs:
  - c++
topic_type:
  - APIRef
  - kbSyntax
api_type:
  - DllExport
api_location:
  - Advapi32.dll
  - API-MS-Win-eventing-Legacy-l1-1-0.dll
  - advapi32legacy.dll
api_name:
  - QueryTrace
  - QueryTraceA
  - QueryTraceW
---

# QueryTraceW function

## -description

The **QueryTrace** function retrieves the property settings and session
statistics for the specified event tracing session.

This function is obsolete. The
[ControlTrace](/windows/win32/api/evntrace/nf-evntrace-controltracew) function
supersedes this function.

## -parameters

### -param TraceHandle

Handle to the event tracing session to be queried, or 0. You must specify a
non-zero _TraceHandle_ if _InstanceName_ is **NULL**. This parameter will be
used only if _InstanceName_ is **NULL**. The handle is returned by the
[StartTrace](/windows/win32/api/evntrace/nf-evntrace-starttracew).

### -param InstanceName

Name of the event tracing session to be queried, or **NULL**. You must specify
_InstanceName_ if _TraceHandle_ is 0.

To specify the NT Kernel Logger session, set _InstanceName_ to
**KERNEL_LOGGER_NAME**.

### -param Properties

Pointer to an initialized
[EVENT_TRACE_PROPERTIES](/windows/desktop/ETW/event-trace-properties) structure.

You only need to set the **Wnode.BufferSize** member of the
[EVENT_TRACE_PROPERTIES](/windows/desktop/ETW/event-trace-properties) structure.
You can use the maximum session name (1024 characters) and maximum log file name
(1024 characters) lengths to calculate the buffer size and offsets if not known.

On output, the structure members contain the property settings and session
statistics for the event tracing session.

**Starting with Windows 10, version 1703:** For better performance in cross
process scenarios, you can now pass filtering into **QueryTrace** for system
wide private loggers. You will need to pass in the new
[EVENT_TRACE_PROPERTIES_V2](/windows/desktop/ETW/event-trace-properties-v2)
structure to include filtering information. See
[Configuring and Starting a Private Logger Session](/windows/desktop/ETW/configuring-and-starting-a-private-logger-session)
for more details.

## -returns

If the function succeeds, the return value is ERROR_SUCCESS.

If the function fails, the return value is one of the
[system error codes](/windows/win32/debug/system-error-codes). The following
are some common errors and their causes.

- **ERROR_BAD_LENGTH**

  One of the following is true:

  - The **Wnode.BufferSize** member of _Properties_ specifies an incorrect size.
  - _Properties_ does not have sufficient space allocated to hold a copy of the
    session name and log file name (if used).

- **ERROR_INVALID_PARAMETER**

  One of the following is true:

  - _Properties_ is **NULL**.
  - _InstanceName_ and _TraceHandle_ are both **NULL**.
  - _InstanceName_ is **NULL** and _TraceHandle_ is not a valid handle.

- **ERROR_ACCESS_DENIED**

  Only users running with elevated administrative privileges, users in the
  Performance Log Users group, and services running as LocalSystem,
  LocalService, NetworkService can query event tracing sessions. To grant a
  restricted user the ability to query trace sessions, add them to the
  Performance Log Users group or see
  [EventAccessControl](/windows/desktop/api/evntcons/nf-evntcons-eventaccesscontrol).

  **Windows XP and Windows 2000:** Anyone can control a trace session.

- **ERROR_WMI_INSTANCE_NOT_FOUND**

  The given session is not running.

## -remarks

Event trace controllers call this function.

This function is obsolete. Instead, use
[ControlTrace](/windows/win32/api/evntrace/nf-evntrace-controltracew) with
_ControlCode_ set to **EVENT_TRACE_CONTROL_QUERY**.

> [!NOTE]
> The evntrace.h header defines QueryTrace as an alias which
> automatically selects the ANSI or Unicode version of this function based on
> the definition of the UNICODE preprocessor constant. Mixing usage of the
> encoding-neutral alias with code that not encoding-neutral can lead to
> mismatches that result in compilation or runtime errors. For more information,
> see
> [Conventions for Function Prototypes](/windows/win32/intl/conventions-for-function-prototypes).

## -see-also

[ControlTrace](/windows/win32/api/evntrace/nf-evntrace-controltracew)

[QueryAllTraces](/windows/desktop/ETW/queryalltraces)
