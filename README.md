# ARP Assignment 2: Drone Simulator with Watchdog

**Author:** Amr Magdy Mohamed Elsayed Abdalla
**ID:** [Your Student ID]
**Date:** December 2025

## Changelog (Corrections from Assignment 1)

In response to the feedback from the first assignment, the following significant corrections have been implemented in this version:

### 1. Physics Engine Stability (Fixes "Teleporting")
* **Correction:** Modified `calc_raw_repulsion` in `output_window.c`.
* **Detail:** Drastically reduced the maximum repulsive force cap from 200.0 to 20.0 and increased the minimum distance clamp from 0.1 to 1.0.
* **Impact:** This prevents the singularity where d -> 0 causes an infinite force spike, ensuring the drone no longer "teleports" when touching walls or obstacles.

### 2. Systematic Debugging (Fixes "No Debug Output")
* **Correction:** Added `log_debug_system()` in `common_utils.c` and instrumented `output_window.c`.
* **Detail:** A structured logging mechanism now captures critical state changes (Force Inputs, Target Hits, Score Updates) to `log/debug_log.txt` with timestamps and PIDs.
* **Impact:** Provides a verifiable trace of the system's internal logic during execution.

### 3. Window Resize Handling (Fixes "Glitches")
* **Correction:** Enhanced `handle_resize` in `output_window.c`.
* **Detail:** The handler now explicitly calls `endwin()` and `clear()` to reset the ncurses buffer and immediately clamps the drone's position to the new `COLS`/`LINES` boundaries.
* **Impact:** Prevents visual artifacts and logic errors where the drone could exist outside the visible window after resizing.

---

## Project Overview
This project is a multi-process drone simulator governed by a Blackboard architecture. It includes a Watchdog process to monitor system health and separate logging for all components.

## Architecture
* **Blackboard (Server):** Central state manager.
* **Watchdog:** Monitors active processes via log file timestamps.
* **Input/Output:** Ncurses-based UI for control and visualization.
* **Targets/Obstacles:** Independent generator processes.

## How to Run
1.  **Compile:** `./compile.sh`
2.  **Run:** `./run.sh`
3.  **Logs:** Check `log/` folder for `debug_log.txt` and `error_log.txt`.
