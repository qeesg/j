curl -X POST "https://proxy.tune.app/chat/completions" \
-H "Authorization: nbx_XDjMI75GLRNRfGoAL7rtvRPEwswHGSZz2ZL" \
-H "Content-Type: application/json" \
-d '{
  "temperature": 0.8,
 
  "messages": [
  {
    "role": "user",
    "content": ""
  }
],
  "model": "rohan/Meta-Llama-3-70B-Instruct",
  "stream": false,
  "penalty": 0,
  "max_tokens": 900
}'
