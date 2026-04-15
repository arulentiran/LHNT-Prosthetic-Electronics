April 02  - Power Problems

**Issue:** 

- 3 DC  motors are moving, 1 is slow
- Boost converter really hot

**That heat can happen for a few common reasons:**

1. **Motor current is too high**
DC motors draw a lot at startup and when stalled. Two motors can easily overload a small boost converter.
2. **The converter is undersized**
Many small boost modules are advertised for “2 A” or similar, but that is often optimistic and not realistic for motors.
3. **Motors create noise and current spikes**
Motors are inductive loads. They can make the converter work much harder, especially if filtering is poor.
4. **Battery cannot supply enough current**
A small 3.7 V battery may sag in voltage under load. Then the boost converter pulls even harder, heats more, and may become unstable.
5. **No proper motor driver setup**
The ESP32 should not power motors directly. The motors should be powered through a **motor driver** with a separate power path.
6. **Stall or mechanical load**
If the motors are stuck, starting under heavy load, or wired in a way that makes them fight resistance, current shoots up fast.

**Diagnostics:**

- Turned the potentiometer to reduce voltage a bit and increase the current
- Boost converter still a bit hot but not as much
- 1 DC motor is not being supplied voltage

**Problems:**

- Boost converter may be overheating/damaged  because;
    - too much output current demanded
    - very high startup/stall current from motors
    - battery voltage sagging, which forces even more input current
    - overheating of the inductor, switch, or chip
    - cheap module not actually rated for the real load
    
    Sometimes it is not fully dead, just **going into protection**:
    
    - thermal shutdown
    - overcurrent limit
    - undervoltage behavior from the battery/input side
- Would need to brainstorm ideas on how to sustainably power all 5 DC motors long-term because of the boost converter cannot sustain the current load demand
- Battery died needs charging

Next Steps:

- Re-do all the connections again and go one by one with the DC motors to make sure they are all properly connected and powered
- Maybe test using a DC power source generator
