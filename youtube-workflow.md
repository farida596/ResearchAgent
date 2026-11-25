# YouTube transcript workflow

## What it does

The YouTube workflow:

1. Validates a supported YouTube URL and extracts its 11-character video ID.
2. Requests existing captions in the preferred language order (English and Arabic by default).
3. If those tracks are unavailable, checks for another available caption track.
4. Preserves caption segment start times and text.
5. Sends the timestamped transcript to the configured LLM for the requested analysis.

The analysis can summarize main ideas, explain difficult concepts, answer questions, and identify important moments. Timestamps must come from the retrieved transcript; the model should not invent them.

## What it does not do

- It does not watch video frames.
- It does not download or transcribe the video's audio.
- It cannot create captions when a video has no accessible caption track.

## Troubleshooting

### No captions or transcript track

The video may have captions disabled, may be private or age-restricted, or may not expose a caption track through the transcript service. Try another public video with captions or provide a transcript export.

### `RequestBlocked` or `IpBlocked`

YouTube may be blocking requests from the hosted runtime's IP address. This is a source/network restriction, not an LLM error. Try running the transcript retrieval locally or supply transcript text manually. Repeated retries from the same blocked environment may not help.

### LLM rate limit or temporary server error

Errors such as HTTP 429, 502, 503, and 504 come from the model provider. The notebook retries some transient failures with backoff. If the provider continues to reject requests, wait and try again or check the provider's status and account limits.

### Separate the two failure stages

Run the notebook's direct caption-retrieval test before running the full analysis. If caption retrieval succeeds but analysis fails, investigate the Groq key, model availability, or provider quota. If caption retrieval fails, changing the LLM provider will not fix the transcript request.
