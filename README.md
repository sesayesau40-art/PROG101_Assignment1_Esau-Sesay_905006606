# PROG101_Assignment1_Esau-Sesay_905006606
Designing logical solutions for Sierra Leone community challenges
**Student:** Esau Sesay | **ID:** 905006606
**Module:** PROG101 Principles of Programming Logic and Design
**Examiner:** Elijah Fullah
**University:** Limkokwing University of Creative Technology, Sierra Leone

## FloodWatch SL
A community flood early-warning system for Freetown. This is a logical design (algorithm, pseudocode and flowcharts), not working code.

## The Problem
Freetown floods almost every rainy season (May to October). Many communities get late, unclear or no warning, and there is no simple shared system that turns local rainfall and water-level readings into a clear alert.

## The Solution
For each monitoring station, a trained volunteer enters:
- Rainfall in the last 3 hours (mm)
- Water level (% of channel capacity)
- Whether the ground is saturated
- Whether the community is flood-prone

The system validates the data, classifies risk as GREEN, YELLOW, ORANGE or RED, escalates for flood-prone areas, sends an SMS alert, logs the record and prints a summary.

## Folder Structure
- `algorithm/algorithm.md` - step-by-step algorithm
- `pseudocode/pseudocode.md` - pseudocode with functions
- `flowcharts/` - Figures 1 to 3 (PNG and DOT files)
- Final report (PDF/DOCX)

## SDG Alignment
- SDG 11 (Sustainable Cities and Communities), Target 11.5
- SDG 13 (Climate Action), Target 13.1
- Supporting: SDG 3 and SDG 1

## Open Source and Digital Public Good
- Intended licence: MIT (OSI-approved)
- Open, transparent thresholds that experts can inspect and improve
- Works on basic phones by SMS, so it is accessible and free for communities
- Privacy-respecting: only opt-in phone numbers, no unnecessary personal data
- Safeguard: invalid data after 3 attempts is flagged for a manual check

## How to Contribute
1. Fork this repository.
2. Make your change (for example, translate alert messages into Krio or suggest better thresholds).
3. Open a pull request, or report problems through GitHub Issues.
## project Author 
<Esau Sesay-DIT1101
>ID:905006606
# Pseudocode: FloodWatch SL

```
PROGRAM FloodWatchSL

// ---------- CONSTANTS ----------
CONSTANT RAIN_WATCH    = 50    // mm of rain in 3 hours
CONSTANT RAIN_WARNING  = 80
CONSTANT RAIN_SEVERE   = 120
CONSTANT LEVEL_WATCH   = 50    // % of drain / channel capacity
CONSTANT LEVEL_WARNING = 75
CONSTANT LEVEL_SEVERE  = 90
CONSTANT MAX_ATTEMPTS  = 3

// ---------- MAIN MODULE ----------
BEGIN
    SET alertCount = 0
    SET manualCount = 0
    SET redCount = 0, orangeCount = 0, yellowCount = 0, greenCount = 0

    OUTPUT "Enter number of monitoring stations:"
    INPUT stationCount

    SET i = 1
    WHILE i <= stationCount DO
        OUTPUT "Enter station / community name:"
        INPUT stationName

        IF GetValidatedReadings(stationName, rain, level, saturated, floodProne) = TRUE THEN
            SET risk = CalculateRiskLevel(rain, level, saturated)
            SET risk = ApplyEscalation(risk, floodProne)

            IF risk <> "GREEN" THEN
                SET message = BuildAlertMessage(stationName, risk, rain, level)
                CALL SendAlert(message)
                SET alertCount = alertCount + 1
            END IF

            CALL LogRecord(stationName, rain, level, risk)
            CALL UpdateCounters(risk)
        ELSE
            SET manualCount = manualCount + 1
            OUTPUT stationName + ": invalid data - MANUAL CHECK NEEDED"
        END IF

        SET i = i + 1
    END WHILE

    CALL DisplaySummary(stationCount, alertCount, manualCount)
END

// ---------- FUNCTION: INPUT + VALIDATION (repetition) ----------
FUNCTION GetValidatedReadings(stationName, BYREF rain, BYREF level,
                              BYREF saturated, BYREF floodProne) RETURNS Boolean
    SET attempts = 0
    SET valid = FALSE

    WHILE valid = FALSE AND attempts < MAX_ATTEMPTS DO
        OUTPUT "Rainfall in last 3 hours (mm):"
        INPUT rain
        OUTPUT "Water level (% of channel capacity):"
        INPUT level
        OUTPUT "Ground saturated? (TRUE/FALSE):"
        INPUT saturated
        OUTPUT "Flood-prone community? (TRUE/FALSE):"
        INPUT floodProne

        IF IsValid(rain, level, saturated) THEN
            SET valid = TRUE
        ELSE
            OUTPUT "Invalid reading - please re-enter"
            SET attempts = attempts + 1
        END IF
    END WHILE

    RETURN valid
END FUNCTION

FUNCTION IsValid(rain, level, saturated) RETURNS Boolean
    RETURN (rain >= 0) AND (level >= 0) AND (level <= 100)
END FUNCTION

// ---------- FUNCTION: RISK CLASSIFICATION (decision structure) ----------
FUNCTION CalculateRiskLevel(rain, level, saturated) RETURNS String
    IF rain >= RAIN_SEVERE OR level >= LEVEL_SEVERE THEN
        RETURN "RED"
    ELSE IF (rain >= RAIN_WARNING AND level >= LEVEL_WATCH) OR level >= LEVEL_WARNING THEN
        RETURN "ORANGE"
    ELSE IF rain >= RAIN_WATCH OR (level >= LEVEL_WATCH AND saturated = TRUE) THEN
        RETURN "YELLOW"
    ELSE
        RETURN "GREEN"
    END IF
END FUNCTION

FUNCTION ApplyEscalation(risk, floodProne) RETURNS String
    IF floodProne = TRUE AND risk = "YELLOW" THEN
        RETURN "ORANGE"
    END IF
    RETURN risk
END FUNCTION

// ---------- FUNCTION: ALERT MESSAGE ----------
FUNCTION BuildAlertMessage(stationName, risk, rain, level) RETURNS String
    IF risk = "RED" THEN
        SET advice = "SEVERE FLOOD RISK. Move to higher ground NOW."
    ELSE IF risk = "ORANGE" THEN
        SET advice = "Flooding likely. Prepare to leave; move children and elderly first."
    ELSE
        SET advice = "Flooding possible. Stay alert and clear drains."
    END IF
    RETURN risk + " ALERT - " + stationName + ": " + advice +
           " (Rain " + rain + "mm, Water " + level + "%)"
END FUNCTION

FUNCTION SendAlert(message)
    OUTPUT "SMS/Alert sent: " + message
END FUNCTION

// ---------- FUNCTION: RECORD KEEPING & SUMMARY ----------
FUNCTION LogRecord(stationName, rain, level, risk)
    APPEND (date, stationName, rain, level, risk) TO floodLog
END FUNCTION

FUNCTION UpdateCounters(risk)
    IF risk = "RED" THEN redCount = redCount + 1
    ELSE IF risk = "ORANGE" THEN orangeCount = orangeCount + 1
    ELSE IF risk = "YELLOW" THEN yellowCount = yellowCount + 1
    ELSE greenCount = greenCount + 1
    END IF
END FUNCTION

FUNCTION DisplaySummary(stationCount, alertCount, manualCount)
    OUTPUT "Stations processed: " + stationCount
    OUTPUT "Alerts sent: " + alertCount
    OUTPUT "RED: " + redCount + "  ORANGE: " + orangeCount +
           "  YELLOW: " + yellowCount + "  GREEN: " + greenCount
    OUTPUT "Manual checks needed: " + manualCount
END FUNCTION
```
 <img width="950" height="1280" alt="WhatsApp Image 2026-11-19 at 18 54 48" src="https://github.com/user-attachments/assets/49063941-f468-4840-8e46-3aa93de2074b" />

