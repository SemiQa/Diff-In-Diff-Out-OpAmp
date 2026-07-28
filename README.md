![](../../workflows/gds/badge.svg) ![](../../workflows/docs/badge.svg)

### Fully-Differential Class-AB Folded-Cascode Operational Amplifier

A full-custom analog Tiny Tapeout submission: a fully-differential operational
amplifier with a folded-cascode core, Class-AB output stage and on-chip
common-mode feedback, implemented in SkyWater SKY130A on a 1.8 V supply.

Everything in this repository — schematic, layout, extraction and
characterisation — was done by hand in the open-source flow (xschem, ngspice,
Magic, netgen, KLayout). **All numbers below come from post-layout
parasitic-extracted simulation; nothing has been measured on silicon yet.**

- Tile size: 2×2 analog
- Analog pins: 6
- Top module: `tt_um_semiqa_diff_opamp`
- Full documentation: [`docs/info.md`](docs/info.md)
![](./docs/Detailed%20Schematic.png)
## Key specifications

Post-layout (R+C extracted), typical corner, 27 °C, `CL` = 1 pF.
Min/Max are the extremes across `tt/ss/ff/sf/fs` and −20 °C to 125 °C.

| Parameter | Min | Typ | Max | Unit |
| --- | --- | --- | --- | --- |
| Open-loop gain, `RL` = ∞ | 89.8 | 100.3 | 108.2 | dB |
| Gain-bandwidth product | 4.98 | 7.58 | 9.47 | MHz |
| Phase margin | 21.3 | 57.0 | 64.3 | ° |
| CMRR, DC @ CM = 0.9 V | 164.6 | 195.6 | 207.8 | dB |
| PSRR+, DC | — | 150.0 | — | dB |
| Input offset voltage, 1σ (Monte Carlo, n = 100) | — | 1.77 | — | mV |
| Slew rate, `RL` = 5 kΩ | — | ±3.35 | — | V/µs |
| THD @ 1 kHz, 1 Vpp | 9.9e-06 | 2.8e-05 | 8.6e-04 | % |
| THD @ 100 kHz, 1 Vpp | 9.5e-05 | 2.2e-03 | 0.083 | % |
| Output swing, single-ended (THD < 1 %) | 0.357 | — | 1.748 | V |
| Output common-mode | 0.913 | 1.049 | 1.159 | V |
| Quiescent current | 239 | 393 | 1101 | µA |
| Power dissipation | — | 0.707 | — | mW |


## Pinout

| TT pin | Signal | Direction | Apply / expect |
| --- | --- | --- | --- |
| `ua[0]` | `Vp` | Input | 0.3 V — PMOS cascode bias |
| `ua[1]` | `Voutm` | Output | negative output |
| `ua[2]` | `Vn` | Input | 1.5 V — Class-AB bias |
| `ua[3]` | `Voutp` | Output | positive output |
| `ua[4]` | `Vinm` | Input | inverting input |
| `ua[5]` | `Vinp` | Input | non-inverting input |

Both bias pins draw negligible current and can be driven from a resistive
divider. All remaining internal bias voltages are generated on-chip.

## How it works

- **Input stage** — complementary NMOS + PMOS pair for a wide common-mode
  input range.
- **Folded cascode** — delivers ~100 dB of open-loop gain without stacking
  devices, which matters at a 1.8 V supply.
- **Class-AB output** — a translinear bias loop lets the output sink and
  source well beyond the quiescent current, so a 1 kΩ differential load is
  driven without slew limiting at audio frequencies.
- **CMFB** — the error pair uses source degeneration
  (2 × `res_high_po_0p35`, L = 17, ≈ 19.3 kΩ) to keep the common-mode loop
  bandwidth below the differential GBW. Without it the CM loop oscillates at
  6.7 MHz; with it the loop is stable in all five corners from −20 °C to
  125 °C.
- **Miller compensation** — `XC1`/`XC2` MiM capacitors (≈ 5 pF) with nulling
  resistors `XR3`/`XR4`.


## Verification

- DRC: clean (Magic, SKY130A).
- LVS: matches uniquely against the schematic netlist (netgen).
- Parasitic extraction: `extract do resistance` + `extract all`, R+C, without
  flattening the hierarchy.
- The GDS in this repository was confirmed geometrically identical to the
  Magic layout by a KLayout XOR.

## Repository layout

| Path | Contents |
| --- | --- |
| `gds/` | Final GDS |
| `lef/` | LEF abstract |
| `src/` | Verilog wrapper and Magic source |
| `docs/` | Project documentation rendered by Tiny Tapeout |
| `test/` | Test collateral |
| `info.yaml` | Tiny Tapeout project metadata |

## Tiny Tapeout

Tiny Tapeout is an educational project that makes it easier and cheaper to get
a custom design manufactured on a real chip. See
[tinytapeout.com](https://tinytapeout.com) and the
[analog specifications](https://tinytapeout.com/specs/analog/).

## License

Apache-2.0. See [`LICENSE`](LICENSE).
