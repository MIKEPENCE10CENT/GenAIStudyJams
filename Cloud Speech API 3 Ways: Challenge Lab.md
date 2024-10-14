# Cloud Speech API 3 Ways: Challenge Lab

## Task 1. Create an API key

```bash
export API_KEY=<YOUR_API_KEY>
```

## Task 2. Create synthetic speech from text using the Text-to-Speech API

```bash
sudo apt-get install -y virtualenv
python3 -m venv venv
source venv/bin/activate
```

Create the JSON file:

```bash
nano synthesize-text.json
```

Use `curl` to synthesize speech:

```bash
curl -H "Authorization: Bearer "$(gcloud auth application-default print-access-token) \
  -H "Content-Type: application/json; charset=utf-8" \
  -d @synthesize-text.json "https://texttospeech.googleapis.com/v1/text:synthesize" \
  > FILE_NAME
```

Create the Python file:

```bash
nano tts_decode.py
```

## Task 3. Perform speech to text transcription with the Cloud Speech API

Edit the `FILE_NAME`:

```bash
nano FILE_NAME
```

Add the following JSON to your file:

```json
{
  "config": {
      "encoding": "FLAC",
      "languageCode": "fr"
  },
  "audio": {
      "uri": "gs://cloud-samples-data/speech/corbeau_renard.flac"
  }
}
```

Use `curl` to recognize speech:

```bash
curl -s -X POST -H "Content-Type: application/json" --data-binary @FILE_NAME \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > FILE_NAME
```

## Task 4. Translate text with the Cloud Translation API

```bash
sudo apt-get update
sudo apt-get install -y jq
```

Set the variables:

```bash
t_text="TEXT_HERE"
from_lang="ja"
to_lang="en"
t_response="FILE_NAME"
```

Use `curl` to translate:

```bash
response=$(curl -s -X POST \
  -H "Authorization: Bearer $(gcloud auth application-default print-access-token)" \
  -H "Content-Type: application/json; charset=utf-8" \
  -d "{\"q\": \"$t_text\"}" \
  "https://translation.googleapis.com/language/translate/v2?key=${API_KEY}&source=${from_lang}&target=${to_lang}")
```

Check for errors and save the translation:

```bash
if [[ $response == *"error"* ]]; then
  echo "Translation API returned an error:"
  echo "$response"
else
  translate=$(jq -r '.data.translations[].translatedText' <<< "$response")
  if [[ -z "$translate" ]]; then
    echo "Translation is empty or null."
  else
    echo "$translate" > "$t_response"
    echo "Translation saved to $t_response:"
    cat "$t_response"
  fi
fi
```

## Task 5. Detect a language with the Cloud Translation API

Set the variable:

```bash
d_text="TEXT_HERE"
```

Decode the text:

```bash
decoded_text=$(python -c "import urllib.parse; print(urllib.parse.unquote('$d_text'))")
```

Use `curl` to detect the language:

```bash
curl -H "Authorization: Bearer "$(gcloud auth application-default print-access-token) \
  -H "Content-Type: application/json; charset=utf-8" \
  -d "{\"q\": [\"$decoded_text\"]}" \
 "https://translation.googleapis.com/language/translate/v2/detect?key=${API_KEY}" \
  -o "FILE_NAME"
```
