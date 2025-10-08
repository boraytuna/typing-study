# Typing Study — Cross-Device Keystroke Experiment

## Overview
This project implements a controlled typing-speed experiment comparing **typing performance across devices (laptop vs. smartphone)** and **age groups (18–22 vs. 40–55)**.  
It was designed and developed by **Boray Tuna Gören** as part of the **CS-258 Machine Learning** course at St. Bonaventure University.

The experiment measures completion time and key typing-pattern metrics (pauses, backspaces, inter-key intervals) using a browser-based logger connected to a **Google Sheets backend**.  
After data collection, the dataset will be analyzed statistically (ANOVA) and with exploratory **machine-learning models** to detect behavioral differences between devices and age groups.

---

## Experiment Flow

1. **Consent & Participant ID**  
   Participants start on either their phone or laptop and provide consent through the landing page.  
   The backend immediately creates a participant entry and **assigns a device order** (see next section).

2. **Phase 1 – Practice + Three Trials**  
   - One unrecorded *practice* sentence is shown first.  
   - Then, three timed trials are recorded and sent to the backend.  
   - After the third recorded round, Phase 1 automatically ends.

3. **Device Handoff**  
   - If the next device differs from the current one, a **handoff page** appears:  
     - On laptops, a **QR code** is displayed (since phones can scan it).  
     - On phones, only a direct link is shown.  
   - Once Phase 1 is completed, the waiting tab on the other device automatically switches to Phase 2.

4. **Phase 2 – Repeat on the Second Device**  
   The same four sentences (1 practice + 3 recorded) are completed again on the second device.

5. **Completion & Reset**  
   After both phases are finished, a thank-you screen appears and the system resets for the next participant.  
   All data remain logged in Google Sheets for later analysis.

---

## How Device Order Is Assigned (Balanced Randomization)

To keep the dataset balanced across both **age groups** and **device orders**, the backend uses a **meta sheet** to track how many participants have been assigned each combination:

| age_group | order           | count |
|------------|-----------------|-------|
| 18–22      | laptop→phone    | 0     |
| 18–22      | phone→laptop    | 0     |
| 40–55      | laptop→phone    | 0     |
| 40–55      | phone→laptop    | 0     |

When a new participant gives consent:
1. The backend checks their reported age group.  
2. It reads the current counts for that group.  
3. It assigns the **less-used order** (e.g., `laptop→phone` if fewer people have that so far).  
4. In case of a tie, the system picks randomly (50 / 50).  
5. The chosen cell’s count is incremented automatically.

This ensures that data remain roughly **balanced across both devices and age groups** without requiring manual control.

---

## How the Backend Works

- The web app communicates with a **Google Apps Script** service that manages three sheets:  
  **participants**, **trials**, and **meta**.  
- Each `trial_summary` submission logs one sentence’s statistics and updates phase-completion flags.  
- When both phases are marked complete, the script timestamps completion (`end_iso`) in the participants sheet.  
- The **meta sheet** continuously maintains balanced order assignment per age group.

---

## Upcoming Machine-Learning & Analysis Component

This version focuses on **data collection and backend automation**.  
After enough data are collected, the next phase will involve analysis and modeling.

**Planned workflow**
1. Compute each participant’s mean typing time per device.  
2. Extract keystroke features:  
   - mean / 95th-percentile inter-key interval  
   - pauses ≥ 500 ms  
   - backspace count  
   - total completion time  
3. Train classifiers (e.g., logistic regression, SVM) to predict:  
   - Device Type (laptop vs. phone) or  
   - Age Group (18–22 vs. 40–55)  
4. Evaluate performance via stratified cross-validation and compare to ANOVA findings.

---

## Statistical Analysis Plan

The inferential analysis will use a **two-way mixed ANOVA** with:
- **Between-subjects factor:** Age Group (18–22 vs. 40–55)  
- **Within-subjects factor:** Device Type (Laptop vs. Phone)

Dependent variable: **mean completion time per sentence block**.  
Exploratory ML will supplement the results by testing whether keystroke-timing features can classify group membership.

---

## Tech Stack
- **Frontend:** HTML, CSS, Vanilla JS  
- **Backend:** Google Apps Script + Google Sheets (REST-style API)  
- **Data Analysis (Planned):** Python (NumPy, Pandas, SciPy, scikit-learn)  
- **Visualization:** Matplotlib / Seaborn (for statistical & ML reports)

---

## Future Improvements
- Continue refining **balanced randomization** (support additional age groups).  
- Add **keystroke-level logging** for richer ML features.  
- Extend ML models to multi-class classification or regression on continuous age.  

---

© 2025 Boray Tuna Gören — St. Bonaventure University CS-258
