# Typing Study — Cross-Device Keystroke Experiment

## Overview
This project implements a controlled typing-speed experiment comparing **typing time across devices (laptop vs. smartphone)** and **age groups (18–22 vs. 40–55)**.  
It was designed and built by **Boray Tuna Gören** as part of the **CS-258 Machine Learning** at St. Bonaventure University.

The experiment records completion time and typing-pattern metrics (pauses, backspaces, inter-key timing) using a lightweight, browser-based logger connected to a Google Sheets backend.  
After data collection, the dataset will be analyzed statistically (ANOVA) and with exploratory **machine-learning models** (e.g., logistic regression or SVM) to predict device type or age group from keystroke-timing features.

---

## Experiment Flow

1. **Consent & Participant ID**  
   Participants start on either their phone or laptop and give consent through the landing page.  
   The backend immediately creates a new participant entry and randomly assigns a device order:  
   - `phone→laptop` or `laptop→phone`.

2. **Phase 1: Practice + Three Trials**  
   - The first sentence is a *practice* round (not stored).  
   - Then three timed trials are recorded and sent to Google Sheets.  
   - After the third round, the participant is automatically advanced.

3. **Device Handoff**  
   - If the next device differs from the one used for consent, the page shows a **handoff screen**:  
     - On laptops, a **QR code** appears (since phones can scan it).  
     - On phones, only the direct link appears.  
   - When the first device finishes Phase 1, the waiting page on the second device automatically transitions to **Phase 2**.

4. **Phase 2: Second Device Trials**  
   The participant repeats the same four sentences (1 practice + 3 recorded).

5. **Completion & Reset**  
   After both phases, the system shows a thank-you message and resets for the next participant.  
   All trial-level data remain logged in Google Sheets for analysis.

---

## How the Backend Works

- The web app communicates with a **Google Apps Script** endpoint that manages three sheets:  
  **participants**, **trials** 
- Each submission (`trial_summary`) updates or appends a row in the trials sheet and marks a phase complete once its third recorded round is submitted.  
- When both phases are done, the script timestamps completion (`end_iso`) in the participants sheet.

---

## Upcoming Machine-Learning & Analysis Component

The current version of this project focuses on **data collection and backend automation**.  
Once enough participant data have been gathered, the next stage will apply **machine learning and statistical modeling** to the dataset.

Planned workflow:

1. **Compute each participant’s mean typing time per device.**
2. **Extract key typing features:**
   - mean and 95th-percentile inter-key interval  
   - pause count ≥ 500 ms  
   - backspace count  
   - total completion time  
3. **Train classifiers** (e.g., logistic regression, SVM) to predict:
   - Device Type (laptop vs. phone), or  
   - Age Group (18–22 vs. 40–55)
4. **Evaluate** performance using stratified cross-validation and compare to ANOVA results.

---

## Statistical Analysis Plan

The inferential analysis will use a **two-way mixed ANOVA** with:
- **Between-subjects factor:** Age Group (18–22 vs. 40–55)  
- **Within-subjects factor:** Device Type (Laptop vs. Phone)

Dependent variable: **mean completion time per sentence block**.  
Exploratory ML will supplement statistical results by testing whether keystroke features can classify group membership.

---

## Technologies Used
- **Frontend:** HTML, CSS, Vanilla JS  
- **Backend:** Google Apps Script + Google Sheets (REST-like API)  
- **Data Analysis (Planned):** Python (NumPy, Pandas, SciPy, scikit-learn)  
- **Visualization:** Matplotlib / Seaborn (for statistical and ML results)

---

## Future Improvements
- Add balanced randomization by age group and device order.  
- Record more fine-grained keystroke logs for ML feature engineering.  
- Extend ML models to multi-class classification or regression on continuous age.

---

© 2025 Boray Tuna Gören — St. Bonaventure University CS-258
