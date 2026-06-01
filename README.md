# Serverless Edge AI Recovery Pipeline

An automated, serverless iOS pipeline that extracts Apple HealthKit time-series metrics, injects live localized weather data, serializes a strict JSON payload, and queries an LLM API to return dynamic, context-aware physiological recovery diagnostics.

## Architecture Overview
[Apple HealthKit] ──(Sleep & HR)──┐
├──> [iOS Shortcuts Engine] ──(Strict JSON)──> [Gemini API] ──> [Markdown Report]
[Apple Weather]  ──(Feels Like)───┘


1. **Extraction (ETL):** On-device HealthKit queries extract overnight resting heart rate average and total sleep duration.
2. **Context Enrichment:** A live location-based API call fetches current local weather metrics (Feels Like temperature/humidity) to account for environmental thermal stress.
3. **Data Serialization:** Standardizes raw health and environmental metrics into a strict JSON payload compliant with the Google Gemini API schema.
4. **LLM Inference & Generation:** Executes an asynchronous REST POST request to the `gemini-3.5-flash` endpoint, returning an immediate, personalized athletic decision matrix.

---

## Technical Stack

*   **Orchestration:** iOS Shortcuts (Edge Automation)
*   **Data Layer:** Apple HealthKit (Time-Series Health Data)
*   **Ingestion & Transport:** RESTful APIs, JSON Serialization, `HTTPS POST`
*   **Inference Engine:** Google Gemini API (`gemini-3.5-flash`)

---

## Deployment & Payload Structure

### 1. API Payload Schema
The pipeline maps device variables directly into the following JSON configuration wrapper:

```json
{
  "contents": [
    {
      "parts": [
        {
          "text": "I slept for [Hours] hours and my average Heart Rate avg is [Heart Rate]. I am 32 years old training for a half marathon and an Ironman 70.3. My schedule today calls for an outdoor run. Considering my endurance training load and the current weather in Flower Mound is [Feels Like], assess my recovery and advise if I should push through the run, adjust my pace and distance, or scale back to active recovery."
        }
      ]
    }
  ]
}
2. Implementation Steps
Clone/Import: Import the pipeline shortcut to your iOS device via the provided iCloud link.

Authentication: Set your Gemini API key as a local variable within the Get contents of URL block header.

Trigger: Execute manually via an iOS Home Screen widget, or bind to a morning automation trigger (e.g., when your daily alarm is turned off).

Sample Diagnostic Output
When successfully executed, the pipeline dynamically renders a structured Markdown report within iOS Quick Look:

Physiological Assessment: Correlates sleep deficits against baseline vagal tone / athletic bradycardia (e.g., 41 bpm sleeping HR).

The Decision Matrix: Provides explicit "Go / No-Go" training pathways (e.g., Option A: Proceed with Pace Adjustments, Option B: Active Recovery, Option C: Complete Rest).

Tactical Adjustments: Evaluates cardiovascular drift and dehydration risk based on real-time temperature and humidity parameters.

Disclaimer: This project is intended as a rapid prototype exploring mobile edge data integration and zero-friction prompt engineering. It does not constitute medical advice.
