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

   [
  {
    "role": "user",
    "content": ""
  }
],
  fetch("https://proxy.tune.app/chat/completions", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "Authorization": "nbx_XDjMI75GLRNRfGoAL7rtvRPEwswHGSZz2ZL",
  },
  body: JSON.stringify({
    "temperature": 0.8,
     
    messages:  [
  {
    "role": "user",
    "content": ""
  }
],
    model: "rohan/Meta-Llama-3-70B-Instruct",
    stream: false,
    "penalty":  0,
    "max_tokens": 900
  })
})import json
import requests

stream = False
url = "https://proxy.tune.app/chat/completions"
headers = {
    "Authorization": "nbx_XDjMI75GLRNRfGoAL7rtvRPEwswHGSZz2ZL",
    "Content-Type": "application/json",
}
data = {
  "temperature": 0.8,
   
    "messages":  [
  {
    "role": "user",
    "content": ""
  }
],
    "model": "rohan/Meta-Llama-3-70B-Instruct",
    "stream": stream,
    "penalty":  0,
    "max_tokens": 900
}
response = requests.post(url, headers=headers, json=data)
if stream:
    for line in response.iter_lines():
        if line:
            l = line[6:]
            if l != b'[DONE]':
              print(json.loads(l))
else:
  print(response.json())from langchain.llms import OpenAI
from langchain.chat_models import ChatOpenAI

chat_model = ChatOpenAI(
    openai_api_key="nbx_XDjMI75GLRNRfGoAL7rtvRPEwswHGSZz2ZL",
    openai_api_base="https://proxy.tune.app/",
    model_name="rohan/Meta-Llama-3-70B-Instruct"
)

out = chat_model.predict("hi!")
print(out)package main

import (
	"bufio"
	"bytes"
	"encoding/json"
	"fmt"
	"net/http"
)

func main() {
	url := "https://proxy.tune.app/chat/completions"
	apiKey := "nbx_XDjMI75GLRNRfGoAL7rtvRPEwswHGSZz2ZL"

	payload := map[string]interface{}{
    "temperature": 0.8,
		"messages":  [
  {
    "role": "user",
    "content": ""
  }
],
		"model": "rohan/Meta-Llama-3-70B-Instruct",
		"stream": false,
    "penalty":  0,
		"max_tokens": 900
	}

	payloadBytes, err := json.Marshal(payload)
	if err != nil {
		fmt.Println("Error marshaling payload:", err)
		return
	}

	req, err := http.NewRequest("POST", url, bytes.NewReader(payloadBytes))
	if err != nil {
		fmt.Println("Error creating request:", err)
		return
	}

	req.Header.Add("Authorization", apiKey)
	req.Header.Add("Content-Type", "application/json")
  

	client := &http.Client{}
	resp, err := client.Do(req)
	if err != nil {
		fmt.Println("Error sending request:", err)
		return
	}
	defer resp.Body.Close()

	fmt.Println("Response:")
	scanner := bufio.NewScanner(resp.Body)
	for scanner.Scan() {
		line := scanner.Bytes()
		if len(line) > 6 {
			if string(line[6:]) != "[DONE]" {
				var jsonData map[string]interface{}
				err := json.Unmarshal(line[6:], &jsonData)
				if err != nil {
					fmt.Println("Error parsing JSON:", err)
				} else {
					fmt.Println(jsonData)
				}
			}
		}
	}
	if err := scanner.Err(); err != nil {
		fmt.Println("Error reading response:", err)
	}
}// Make sure to add these dependencies to your Cargo.toml:
// [dependencies]
// reqwest = "0.11"
// serde = { version = "1.0", features = ["derive"] }
// serde_json = "1.0"

use std::io::{self, BufRead};
use reqwest::blocking::{Client, RequestBuilder};
use serde_json::Value;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let url = "https://proxy.tune.app/chat/completions";
    let api_key = "nbx_XDjMI75GLRNRfGoAL7rtvRPEwswHGSZz2ZL";

    let payload = json!({
      "temperature": 0.8,
        "messages": [
  {
    "role": "user",
    "content": ""
  }
],
        "model": "rohan/Meta-Llama-3-70B-Instruct",
        "stream": false,
        "penalty":  0,
        "max_tokens": 900
    });

    let client = reqwest::blocking::Client::new();
    let response = send_request(&client, url, api_key, payload)?;

    println!("Response:");
    for line in io::BufReader::new(response).lines() {
        let line = line?;
        if line.len() > 6 && &line[..6] != "[DONE]" {
            let json_data: Value = serde_json::from_str(&line[6..])?;
            println!("{:?}", json_data);
        }
    }

    Ok(())
}

fn send_request(
    client: &Client,
    url: &str,
    api_key: &str,
    payload: serde_json::Value,
) -> Result<reqwest::blocking::Response, reqwest::Error> {
    let request = client.post(url)
        .header("Authorization", api_key)
        .header("Content-Type", "application/json")
        
        .json(&payload);

    request.send()
}
