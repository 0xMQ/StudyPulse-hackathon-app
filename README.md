StudyPulse

A weekly wellness check-in for college students. Answer five quick questions and get a short, friendly reflection plus two habit suggestions, generated with the OpenAI API.

Built for the AI Adventure Hackathon 2026 at Cal Poly Pomona.

StudyPulse is not a therapist, counselor, or medical tool. It gives general, non-clinical encouragement only. If you're struggling, reach out to your campus counseling center. In the US, you can also call or text 988 for immediate support.


How it works
A student rates their week on five inputs (below).
The front end sends the answers as JSON to the /reflect endpoint.
The Flask backend builds a prompt and calls the OpenAI Chat Completions API (gpt-4o-mini).
The app returns a response of under 120 words: a 2-3 sentence reflection, then two habit suggestions under "💡 Try this week:".
Inputs
Field	Meaning	Scale
sleep	Average hours of sleep per night	4-10 hours (default 7)
workload	Academic workload	1-5 (5 = overwhelming)
stress	Stress level	1-5 (5 = very stressed)
social	Social and fun time	1-5 (5 = plenty)
movement	Movement and exercise	1-5 (5 = very active)

Missing values fall back to defaults (7 hours of sleep, 3 for everything else).

AI guardrails

The system prompt keeps the assistant in a supportive, non-clinical lane:

Never diagnoses a mental health condition.
Never suggests medication.
Never provides crisis counseling.
Gently mentions campus counseling when the answers suggest severe distress (high stress, low sleep, low social time).
Uses a warm, friend-like tone and a fixed response format and length.
Getting started

Prerequisites: Python 3.8+ and an OpenAI API key.

bash
git clone https://github.com/0xMQ/hackathon-app.git
cd hackathon-app
pip install flask

# macOS / Linux
export OPENAI_API_KEY="your-key-here"

# Windows (PowerShell)
$env:OPENAI_API_KEY="your-key-here"

python app.py

Then open http://localhost:5000.

The API key is read from the OPENAI_API_KEY environment variable. Never commit a key to the repository.

API
POST /reflect

Request:

json
{
  "sleep": 6,
  "workload": 4,
  "stress": 4,
  "social": 2,
  "movement": 3
}

Success response (200):

json
{ "reflection": "<generated reflection and habit suggestions>" }

Failure response (500):

json
{ "error": "<error message>" }
Project structure
app.py               Flask app: serves the page and handles /reflect
templates/
  index.html         Front-end check-in form
Privacy

StudyPulse has no accounts and no database, and it doesn't store check-in data. The answers are sent to the OpenAI API to generate each reflection and are handled under OpenAI's terms.

Limitations
Requires an internet connection and a valid OpenAI API key. Each check-in makes one API call.
Reflections are general and non-clinical. They are not a substitute for professional care.
