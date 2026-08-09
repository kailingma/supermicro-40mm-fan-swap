<img src="https://github.com/kailingma/supermicro-40mm-fan-swap/blob/fa2301d3958467b394805c631295f65bca7e54d1/hack%20club%20macondo%20logo.png" height=100>

A [Hack Club](https://hackclub.com) [Macondo](https://macondo.hackclub.com) project

# Supermicro 40mm Fan Swap

[Onshape](https://cad.onshape.com/documents/373ffd8f172fee48bdac2120/w/7900feb72fc626060cd19447/e/43b79a1c3cb5936e74e0c8c3?renderMode=0&uiState=6a223409dba78bde51dcc5cb) | [Macondo](https://macondo.hackclub.com) | [BOM](https://github.com/kailingma/supermicro-40mm-fan-swap/blob/main/BOM.csv) | [STEP](https://github.com/kailingma/supermicro-40mm-fan-swap/blob/main/Noctua%2040mm%20Fan%20Spacer.step)

## What and why

My 2U Supermicro server is loud, and the worst offender is the fan in its 1U power supply. This project yanks that fan out and replaces it with two Noctua 40mm fans on a 3D-printed adapter that fits the original opening. Same airflow, way less noise.

## How it fits together

The fans bolt onto the adapter 15mm apart. Funny enough, 15mm is the textbook gap for a 120mm fan, so at 40mm scale it's about three times the ideal spacing, which means the airflow from the first fan evens out before it hits the second one. The adapter drops straight into the PSU housing where the original fan lived.

The electrical side is where the real work is. The original fan cable was cut out, the connector is spared from a donor power supply, and everything is joined with Western Union splices, soldered, and heat shrunk.

## Parts

- 1U Supermicro power supply (from a 2U server)
- 2x Noctua 40mm fans, with the splitter cable they ship with
- 3D-printed adapter (second prototype)
- Wire, solder, hot glue, heat shrink

## Images

| ![Final Wiring](https://cdn.hackclub.com/019fa5c5-87a2-711f-b64f-dece37f7a3b3/IMG_8336.jpeg) | ![Western Union Splice](https://cdn.hackclub.com/019e9885-b213-788e-bbb1-da07c3d5a32a/7E6FA617-C8F8-44AD-B1E0-4BC1412E9928_1_105_c.jpeg) |
|-------|--------|
| Final Wiring | Western Union Splice |
| ![Soldering Job](https://cdn.hackclub.com/019e9889-5cb2-7120-85d8-92242b3def34/IMG_8036.jpeg) | ![Fan Comparison](https://cdn.hackclub.com/019e95ca-c308-7616-87bb-e83dbb8ae7cb/22B9442E-413E-4AB7-931C-E74A04B14E89_1_102_o.jpeg)
| Soldering Job | Fan Comparison |

## Wiring Diagram
```mermaid
flowchart LR
    subgraph PSU["PSU fan connector (donor supply)"]
        direction TB
        P_V["+12V"]
        P_G["GND"]
        P_T["TACH"]
        P_P["PWM"]
    end

    subgraph SPL["4x splice, soldered + heat shrunk"]
        direction TB
        S_V["+12V"]
        S_G["GND"]
        S_T["TACH"]
        S_P["PWM"]
    end

    subgraph F1["Fan 1"]
        direction TB
        F1V["+12V"]
        F1G["GND"]
        F1T["TACH"]
        F1P["PWM"]
    end

    subgraph F2["Fan 2"]
        direction TB
        F2V["+12V"]
        F2G["GND"]
        F2T["TACH"]
        F2P["PWM"]
    end

    P_V --- S_V
    P_G --- S_G
    P_T --- S_T
    P_P --- S_P

    S_V --- F1V
    S_V --- F2V
    S_G --- F1G
    S_G --- F2G
    S_T --- F1T
    S_T --- F2T
    S_P --- F1P
    S_P --- F2P
```

## Build log

1. **Modelling (Jun 4):** measured the fans, settled on wall thickness, printed two prototypes. Also learned that my 15mm gap is the textbook spacing for a 120mm fan, so at 40mm scale the airflow evens out nicely between the two fans.
2. **Soldering (Jun 5):** first time trying the Western Union splice, on tiny AWG 24 and 22 wires. They came out clean, though I picked the wrong heat shrink size for the first one and forgot it entirely, so it's a bit ugly.
3. **Final wiring and test (Jul 27):** everything soldered, shrink wrapped, and assembled. On power-up the fans spin for a moment and the PSU shuts itself off.


## Result

I never got a successful test. The fans spin up briefly, then the power supply trips its overcurrent protection and shuts off, a fault the donor supply has had for years. Most likely the donor fan cabling was defective from the start, and it's possible the PWM and TACH wires were pinned wrong too. Without a known-good supply I couldn't isolate which one.

The wiring itself is solid and the adapter fits. If I pick this back up, the next step is checking the pinout against the supply and testing with cables from a known good power supply.
