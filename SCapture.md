

# Introduction #

[SCapture.pas](http://code.google.com/p/robstechcorner/source/browse/trunk/Delphi/utils/SCapture.pas) has routines to capture screen shots from multiple monitors.


# TCaptureContext Enum #

  * cActiveWindow: Capture an image of the ActiveWindow only.
  * cDesktopMonitor: Capture an image of the first / primary monitor.  Avoids MultiMon calls, so it would be faster than ccSpecificMonitor with index of 1
  * cActiveMonitor: Capture an image of the monitor with the active window on it.  If the active window is on more than one monitor it will pick the monitor that has the larger portion of the window on it.   If the active window is not in the visible space of any monitor, it will pick the closest one.
  * cSpecificMonitor: Capture an Image of a Specific Monitor specified by the aMonitorNum parameter.  Note: aMonitorNum the first monitor is 1.   You can use the MonitorCount function to determine the valid range of monitors. An EMonitorCaptureException will occur if you select a monitor number outside the valid range.
  * cAllMonitors: Capture a single image of all the monitors.

# CaptureScreen() #

```
procedure CaptureScreen(aCaptureContext : TCaptureContext; destBitmap : TBitmap;aMonitorNum : Integer = 1); 
```

Captures the screen into destBitmap based on specified Capture Context.

Example of usage
```
var
  b : TBitMap;
begin
  b := TBitmap.Create; 
  try 
    CaptureScreen(ccAllMonitors,B,1);
    Image1.Picture.Assign(b);
  finally
    b.FreeImage;
    b.Free; 
  end;
end;
```

# CaptureRect() #

```
procedure CaptureRect(aCaptureRect : TRect;destBitMap : TBitmap); inline; 
```

Capture the area of the screen specified by the aCaptureRect Parameter.

Example of usage
```
var
  b : TBitMap;
  r : TRect;
begin
  b := TBitmap.Create; 
  try 
    r.top := 20;
    r.left := 20;
    r.bottom := 20;
    r.right := 20;
    CaptureRect(R,B);
    Image1.Picture.Assign(b);
  finally
    b.FreeImage;
    b.Free; 
  end;
end;
```

# CaptureDeviceContext() #

```
procedure CaptureDeviceContext(SrcDC: HDC;aCaptureRect : TRect;destBitMap : TBitmap); inline;
```

This procedure is in the interface, just in case you want to capture from a Device Context that is not the screen, or you already have the Device Context Handle.

See CaptureScreen implemenation for an example of usage.


# EMonitorCaptureException #

```
EMonitorCaptureException = class(Exception); 
```

This exception is raised if selecting a monitor by Index that does not exist.

# MonitorCount() #
```
function MonitorCount : Integer;  
```

> Return the Number of Monitors user has. This can change during a windows Session for example: Remote Desktop Session, Laptop/Projector Settings



# GetMonInfoByIdx() #

```
function GetMonInfoByIdx(MonIdx : Integer) : MONITORINFO; 
```

Returns the [MONITORINFO](http://msdn.microsoft.com/en-us/library/dd145065(VS.85).aspx) structure, based on the monitor index specified.    This ia a 1 based Index.