# Polyfuse

Polyfuses—better known in datasheets as **PPTC (Polymeric Positive‑Temperature‑Coefficient) resettable fuses**—are current‑limiting parts that act like self‑healing thermal switches: they sit at milliohm‑level resistance in normal use, heat up and “open” (switch to kilohms) when a fault pushes too much current, then cool down and reset once power is removed or the fault clears.citeturn1search2 The sections below unpack what that means in practice, step by step, with a focus on the concepts beginners most often struggle with.

---

## 1 What a Polyfuse Is

A polyfuse is a **blob of semi‑crystalline polymer mixed with carbon black particles** sandwiched between two electrodes.citeturn1search0turn1search4 At room temperature the polymer is dense, the carbon grains touch, and the device behaves like a low‑value resistor. When current rises, **I²R self‑heating softens the polymer, it expands, the carbon chains pull apart, and resistance jumps by several orders of magnitude, throttling the fault current to a trickle**.citeturn1search6 Because nothing fuses permanently, the part cools and contracts after power is removed, re‑establishing the carbon paths—hence “resettable.”citeturn0search4

---

## 2 Key Electrical Ratings

| Datasheet term | What it means | Beginner tip |
|----------------|---------------|--------------|
| **I<sub>HOLD</sub>** | Maximum steady‑state current the part can carry indefinitely at 25 °C without tripping.citeturn0search0 | Choose **≥ your normal peak load**. |
| **I<sub>TRIP</sub>** | Minimum current that will guarantee a trip at 25 °C.citeturn0search0 | Must be **≤ the worst‑case fault** you want to protect against. |
| **R<sub>INITIAL</sub> / R<sub>TRIPPED</sub>** | Resistance before and after a trip; expect a 10 mΩ part to rise to kΩ when hot.citeturn1search1 | Rises slightly with every trip—design for margin. |
| **V<sub>MAX</sub>** | Highest DC voltage the device can tolerate without arcing.citeturn1search3 | Stay below this, especially on battery stacks. |
| **I<sub>MAX</sub>** | Peak fault current it can tolerate during the first few milliseconds of a short.citeturn1search3 | Check if your supply can deliver more; add series impedance or a fast fuse if needed. |

**Hold vs Trip confusion:** aim for an I<sub>HOLD</sub> comfortably above your *continuous* current and let I<sub>TRIP</sub> sit below the damage threshold of what you’re guarding. A part with 2 A hold / 4 A trip is fine to protect a 1 A load; it will stay invisible in normal use yet react well before the wiring or ICs cook.citeturn0search2

---

## 3 What Happens During a Fault

1. **Over‑current begins.**  
2. **Self‑heating accelerates (positive feedback).** Polymer hits its “knee” temperature in tens to hundreds of milliseconds.citeturn1search0  
3. **Resistance skyrockets.** Current collapses to a few milliamps—enough to keep the device hot but harmless to the rest of the circuit.citeturn1search2  
4. **Reset phase.** Remove power or clear the short; the device cools back below the knee temperature in seconds and returns to its low‑R state.citeturn1search1  

Because residual current still flows while tripped, a PPTC does **not** disconnect the load as completely as a cartridge fuse; sensitive ICs may still see a few volts.citeturn0search5

---

## 4 Why Use One? (Pros & Cons)

| ✔ Advantages | ✘ Limitations |
|--------------|--------------|
| Automatically resets—no service truck or blown‑fuse RMA.citeturn0search5 | Not crowbar‑fast; a hard short can see full current for several ms. |
| Cheap, tiny (0603 ‑ 1812), surface‑mountable.citeturn1search1 | Resistance rises a little after every trip (aging). |
| Ideal for consumer ports (USB, HDMI, battery packs) where nuisance shorts are common.citeturn0search3 | Ambient temp matters: I<sub>HOLD</sub> derates ~‑6 % / °C above 25 °C, so parts may nuisance‑trip in hot enclosures.citeturn1search3 |
| Acts as a slow inrush limiter because R increases with temperature.citeturn1search4 | Cannot protect against high‑energy lightning or very high voltages; combine with TVS or MOV if needed.citeturn1search7 |

---

## 5 Typical Applications You’ll Recognise

* **USB and USB‑C ports** on PCs, hubs, and dev‑boards (often an 0805‑sized 0.75 A/1.5 A polyfuse next to each port).citeturn0search3  
* **Li‑Ion battery packs**—PPTCs in series with cells provide a “second‑level” over‑current cutback aside from the protection IC.citeturn1search0  
* **Motor driver breakout boards** where stalled motors can pull double their rated current.citeturn0search0  
* **Telecom lines, alarm loops, and HVAC control wiring** where resettable protection avoids truck rolls.citeturn1search1  

---

## 6 How to Pick the Right Part (Beginner Checklist)

1. **List the normal max load current (I<sub>LOAD‑max</sub>).**  
2. **Choose I<sub>HOLD ≥ I<sub>LOAD‑max</sub></sub> at the hottest ambient you expect.** Derating curves are in every datasheet.citeturn1search1  
3. **Verify I<sub>TRIP</sub> < damage threshold.** For USB, the spec says port power must drop after 500 mA for legacy or 900 mA for USB 3.x.  
4. **Check V<sub>MAX</sub>.** Stay below it with plenty of headroom if you have inductive kickback.citeturn1search3  
5. **Confirm I<sub>MAX</sub> & fault profiles.** If your bench supply can dump 30 A into a short, pick a PPTC whose I<sub>MAX</sub> or energy rating covers that.  
6. **Footprint & heat‑spreading.** Larger bodies trip faster (more polymer) but occupy board space; fountains of copper under the pads help cooling and reset time.citeturn1search0  

---

## 7 Board‑Layout & Usage Tips

* **Place close to the power entry** so downstream traces never see the full fault current path.  
* **Avoid burying under airtight shields**; the device needs airflow to cool and reset.  
* **Keep sense resistors or current‑shunt paths downstream**—voltage drop across the polyfuse can skew low‑side measurements if you put the shunt ahead of it.  
* **Label it “F” on the silkscreen** so technicians know it’s deliberate, not a resistor.  

---

## 8 Common Beginner Questions

| Question | Short answer |
|----------|--------------|
| *Does a polyfuse trip instantly like a breaker?* | No; expect 10 ms – 1 s depending on current and part size.citeturn1search0 |
| *Can I press a reset button to speed recovery?* | Removing power is the only “reset button”; forced cooling (fan blast) helps but isn’t required. |
| *Why is there still some voltage on the rail after trip?* | The part sits at high but finite resistance (kΩ), letting a few mA leak. Sensitive logic may brown‑out instead of shutting off completely.citeturn0search5 |
| *Will it wear out?* | Many thousands of trip cycles are typical, but R<sub>INITIAL</sub> creeps upward 10 – 40 % over life.citeturn1search6 |

---

## 9 Take‑away

Think of a **polyfuse as a self‑resetting, temperature‑driven current limiter**. It costs pennies, sits transparently in your circuit, and sacrifices a bit of voltage drop to save your PCB from dead shorts. Pick one by matching its **hold current to your load** and its **trip current to your fault limit**, mind the derating curves, and place it right at the power entry. From Raspberry Pi USB jacks to 100 Ah battery packs, that same principle of polymer expansion keeps smoke—and service calls—out of your projects.