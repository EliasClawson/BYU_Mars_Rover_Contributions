# Quick Reference Guide: Rover Antenna System Design (900 MHz)
## Problem Overview
Reliable communication is critical for rover operation. When the connection slows or fails, it impacts every rover task. Specifically, we need to address multipath fading, signal attenuation, and polarization mismatch issues.

## Definitions
- Multipath Fading: Occurs when transmitted signals reach the receiving antenna by multiple paths due to reflection, diffraction, or scattering off terrain and objects, causing interference and fluctuations in signal strength.
- Signal Attenuation: The reduction in *signal power* as radio waves propagate through the environment, caused by distance, obstacles, terrain, or atmospheric conditions. Usually proportional to available throughput.
- Polarization Mismatch: Happens when transmitting and receiving antennas have misaligned polarization (orientation of the electric field), leading to significantly weaker received signals due to poor alignment. The Yagi antenna at the base station is dual polarized, which helps with this.
- Available Throughput: The maximum data rate that the communication link can support under current signal conditions (e.g. signal strength, noise, interference). It represents the upper limit of how much data could be transmitted.
> **Note:** In practice, the actual throughput is often much lower than what is available with the current connection. This is mostly becuase of the way ROS handles message passing between nodes and introduces delays and overhead, which slows down communication even when the signal is strong.

## Antenna types
#### 1. Monopole (Quarter-wave)
- **Pattern:** Omnidirectional horizontal coverage; donut-shaped pattern.
- **Gain:** Low (~1-2 dBi)
- **Advantages:** Simple, uniform coverage
- **Drawbaks:** Limited gain, weaker at angles (such as when climbing a hill)
#### Monopole Radiation Pattern:
<img src="media/Monopole.jpg" alt="Monopole Radiation" width="300"/>


#### 2. Dipole (Half-Wave) (Chosen Solution)
- **Pattern:** Omnidirectional in azimuth; similar to monopole.
- **Gain:** Moderate (~2 dBi typical).
- **Advantages:** Reliable omnidirectional coverage, polarization diversity (when using dual vertical dipoles).
- **Drawbacks:** Limited gain compared to directional antennas.
#### Dipole Radiation Pattern:
<img src="media/Dipole1y.jpg" alt="Dipole Radiation" width="300"/>

#### 3. Collinear Array
- **Pattern:** Omnidirectional but flattened vertically; increased horizontal gain.
- **Gain:** Moderate-to-high (~5–9 dBi depending on elements).
- **Advantages:** Higher gain extending range.
- **Drawbacks:** Narrow vertical coverage problematic in hilly terrain; less robust against rover tilting.
#### Collinear Radiation Pattern:
<img src="media/Collinear.jpg" alt="Collinear Radiation" width="300"/>

#### 4. Yagi-Uda Antenna (Used at Base Station)
- **Pattern:** Highly directional; narrow beamwidth.
- **Gain:** High (~6–16 dBi depending on elements).
- **Advantages:** Excellent range and throughput when aligned.
- **Drawbacks:** Requires precise alignment; impractical for mobile rover.
#### Yagi Radiation Pattern:
<img src="media/Yagi.jpg" alt="Yagi Radiation" width="300"/>

#### 5. Helical Antenna
- **Pattern:** Directional; can provide circular polarization.
- **Gain:** Moderate (~5–12 dBi).
- **Advantages:** Reduces polarization mismatch; resilient to rover orientation changes.
- **Drawbacks:** Requires base station adaptation; moderate complexity; slight inherent gain loss.
#### Helical Radiation Pattern:
<img src="media/Helical.jpg" alt="Helical Radiation" width="300"/>

#### 6. Patch Antenna
- **Pattern:** Directional, moderate beamwidth.
- **Gain:** Moderate (~6–9 dBi single patch).
- **Advantages:** Compact and directional.
- **Drawbacks:** Requires careful orientation; poor performance if rover orientation varies significantly.
#### Patch Radiation Pattern:
<img src="media/Patch.png" alt="Patch Radiation" width="300"/>

## Frequency Selection (900 MHz vs. 2.4 GHz):
- 900 MHz Chosen: Longer wavelength penetrates terrain better, superior in multipath and non-line-of-sight (NLOS) environments.
- 2.4 GHz Rejected: Higher path loss, poor penetration in rough terrain, reduced performance in NLOS conditions.

## Final Antenna Configuration:
**Dual vertical omnidirectional antennas**
<img src="media/RoverAntennas.JPG" alt="Monopole Radiation" width="400"/>

**Mounting:** Both antennas placed vertically on rover mast (that fold to pigtails). They can slide to adjust spacing, but 33cm (1 wavelength) is the most ideal distancing.

**Reasoning:** Balances omnidirectional coverage, polarization diversity, mechanical simplicity, and resistance to multipath fading.

### Antenna Specifications
The current antenna we use is an [omnidirectional 8dBi fiberglass, outdoor antenna](https://www.data-alliance.net/antenna-900mhz-8dbi-omni-directional-w-n-female-pole-mount-gsm/). It is a series of seven arrays to achieve a higher gain than a single half-wave dipole. Its radiation pattern looks similar to a dipole, but flatter (stronger in horizontal direction but less strength in vertical direction). Essentially, the antenna will perform better than a standard dipole in flat level terrain but worse in hilly terrain.

## Future considerations:
These large fiberglass antennas are fantastic, but it's possible that there are even better ones out there. Any stronger omnidirectional would probably add too much noise, but something directional like a Yagi or patch antenna could be fantastic, *if you can figure out how to orient it towards the base station at all times*. The current placement of the antennas works well, but if you're looking for ways to improve, research polarization diversity. It's possible that a little bit of performance could be gained by orienting one antena at 90 degrees (horizontall back) or placing it vertically on the base of the rover (so it's lower than the one on the mast). 
