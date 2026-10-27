# FreeLipSync AI Starter (Gujarati)

આ ZIP માં Android-friendly web UI અને API integration stub છે. **આ હજી સંપૂર્ણ AI renderer નથી.**
AI model/checkpoint અને ચલાવવા માટે યોગ્ય compute ઉમેર્યા પછી જ વાસ્તવિક વીડિયો બનશે.

## શું સામેલ છે
- `frontend/index.html`: ગુજરાતી UI; Photo-to-video અને Video lip-sync મોડ; audio upload; aspect ratio અને quality પસંદગી.
- `backend/main.py`: FastAPI API stub. `/health` ચાલે છે; `/generate` હજી `501` આપે છે જ્યાં સુધી AI renderer જોડાય નહીં.

## Android પર UI તપાસવું
1. ZIP extract કરો.
2. `frontend/index.html` Chromeમાં ખોલો.
3. ફાઇલ પસંદગી અને UI ચકાસો. Backend URL ખાલી રાખશો તો તે AI જનરેશન સક્રિય નથી તે બતાવશે.

## Backend developer માટે
Python 3.10+ પર:
```bash
cd backend
pip install -r requirements.txt
uvicorn main:app --host 127.0.0.1 --port 8000
```
Health check: `http://127.0.0.1:8000/health`

`/generate` ને વાસ્તવિક renderer સાથે જોડો. APIએ `multipart/form-data` માં `visual`, `audio`, `mode`, `aspect_ratio`, `quality` સ્વીકારવા જોઈએ અને MP4 bytes અથવા JSON `{ "video_url": "https://..." }` પરત આપવું જોઈએ. Productionમાં upload size/type validation, authentication, rate limiting, private storage, automatic file deletion, HTTPS અને CORS restrictions ઉમેરો. જાહેર server પર user-uploaded files અસુરક્ષિત રીતે ન રાખો.

## મફત AI અંગે મહત્વની નોંધ
- મફત compute અવિરત/અમર્યાદિત હોવાની ખાતરી નથી.
- Google Colabના managed runtimesમાં deepfake creation પ્રતિબંધિત છે; તેને આ app માટે hosting loophole તરીકે વાપરશો નહીં.
- Wav2Lip આધારિત કેટલીક Python packagesના license terms personal/research/non-commercial use સુધી મર્યાદિત હોઈ શકે છે. Model code અને weightsની license અલગથી તપાસો; commercial distribution પહેલાં કાનૂની સમીક્ષા કરાવો.
- ફક્ત પોતાની અથવા સ્પષ્ટ સંમતિ ધરાવતી વ્યક્તિના ફોટા/વીડિયો વાપરો.

## API contract
POST `/generate` multipart fields:
- `visual`: image or video
- `audio`: audio file
- `mode`: `photo` or `video`
- `aspect_ratio`: `9:16`, `16:9`, `1:1`
- `quality`: `720p` or `1080p`

Success response: `video/mp4` bytes OR JSON `{ "video_url": "https://..." }`.
