# Services and Costs

## Deepgram (SST)

- Model: Pay As You Go
- Nova-2: $0.0059/min

## Twilio (Voice Provider + TTS)

- To Make Calls: $0.0140/min
- TTS: $0.00080/Price per 100 characters

## OpenAI

- Model: gpt-4-turbo
  - Input: US$0.0100/1K tokens
  - Output: US$0.0300/1K tokens
    `NOTE: AVG: 1000 tokens = 750 words`

## Guestimation

Avg success call duration: `10 mins`
GPT Prompt token : `1500 tokens` [FIXED]
Avg llm token consumption during success calls: `2000-3000 tokens`
=> Customer Rep and Voice Bot interaction => `500-1500 tokens` [Deducting prompt token from avg llm token]
=> STT + TTS = `500-1500 tokens`

Assuming equal number of total word interaction between customer rep and voice bot =>
STT = TTS = `250 - 750 tokens`
STT = `1000-3000 characters` [250 tokens = ~1000 characters]

1. Deepgram SST (Speech-to-Text) cost: $0.0059 * 10 = `0.059$`
2. Twilio Voice Provider: $0.0140 * 10 = `0.14$`
3. Twilio TTS (Text-to-Speech): (0.00080$/100 characters * (1000 - 3000 characters)) = `0.008$ - 0.024$`

4. Open AI costs:
   GPT Prompt cost = 0.01*1500/1000 = `0.015$`
   transcription processing : 0.01 * (250 - 750 tokens)/1000 = `0.0025-0.0075$`
   text generation cost : 0.03 \* (250 - 750 tokens)/1000 = `0.0075 - 0.0225$`

   Total Open AI Costs : Adding all up: `0.025 - 0.045$`

Total cost per call: Adding all up: `0.232 - 0.268$`
