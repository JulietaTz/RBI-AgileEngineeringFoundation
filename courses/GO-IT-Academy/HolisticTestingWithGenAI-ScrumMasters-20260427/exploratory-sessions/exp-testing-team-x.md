# Example Session Sheet

## CHARTER
Analyze MapMaker's View menu functionality and report on areas of potential risk.

## TESTER
Hans Bachler

## TEST NOTES
I touched each of the menu items below, but focused mostly on zooming behavior with various combinations of map elements displayed

### View
- Welcome Screen

### Map Elements:
- Highway Levels
- Street Levels
- Airport Diagrams

## RISKS
- Incorrect display of a map element 
- Incorrect display due to interrupted repaint 
- CD may be unreadable 

## BUGS

### BUG 1321
Zooming in makes you put in the CD 2 when you get to a certain level of granularity (the street names level) even if CD 2 is already in the drive.

## Summary
Many defects. Version not ready for release.
