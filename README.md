

## **Comparison and Results - Speech-to-Text**
The comparison involved conducting three separate recordings, each designed to represent different aspects of a conversation one might have with WALI. These recordings were then transcribed using multiple speech-to-text technologies to assess their performance.  

For **streaming transcription**, the **final transcript** was tracked in real time as the recording progressed, ensuring that the transcription reflected the spoken words without post-processing.  

For **Faster Whisper**, two different model sizes—**small** and **medium**—were implemented. These models transcribed the recordings after completion, allowing for a comparison of accuracy and processing speed between them.  

To ensure consistency across all transcription methods, **all audio files were recorded at a fixed sample rate of 48 kHz**. A sample rate of 48 kHz means that the audio signal was captured **48,000 times per second**, providing a higher resolution of sound, which can improve transcription accuracy by preserving more details of the speech.  

For **streaming transcription**, a **FRAMES_PER_BUFFER** value of **4096** was used. This parameter determines the number of audio frames processed at a time before being sent for transcription. A larger buffer size like 4096 helps balance real-time performance with stability by reducing the number of small, frequent transmissions, which can be computationally expensive, while still allowing for near-instantaneous transcription.  

Both **Faster Whisper** models ran on **CUDA**, which enabled GPU acceleration for faster processing. This allowed the models to handle speech-to-text conversion more efficiently than a CPU, improving speed while maintaining accuracy.  

By analyzing the results across these different transcription methods, the study evaluates their performance in terms of **accuracy, latency, and real-time capability**.  

### Ground Truth: "Let's make a quote for ten starter licenses."

| Method                | Transcript                                      | Latency  | Accuracy  |
|-----------------------|------------------------------------------------|---------|-----------|
| **Streaming**         | Let's make a quote for ten starter licenses.   | real-time | 100%  |
| **AssemblyAI**        | Let's make a quote for 10 starter licenses.    | 6.06s   | 100%  |
| **Faster Whisper Small**  | Let's make a quote for 10 starter licenses.  | 0.29s   | 100%  |
| **Faster Whisper Medium** | Let's make a quote for 10 starter licenses.  | 0.65s   | 100%  |


### Ground Truth: "My quote is good and final. Please print me the PDF. Thank you."  

| Method                | Transcript                                                | Latency  | Accuracy  |
|-----------------------|----------------------------------------------------------|---------|-----------|
| **Streaming**         | My quote is good and final. Please print me the PDF. Thank you. | real-time | 100%  |
| **AssemblyAI**        | My quote is good and final. Please print me the PDF. Thank you. | 6.22s   | 100%  |
| **Faster Whisper Small**  | My quote is good and final. Please print me the PDF. Thank you. | 0.41s   | 100%  |
| **Faster Whisper Medium** | My quote is good and final. Please print me the PDF. Thank you. | 0.64s   | 100%  |


### Ground Truth: "Yes. The details for the deal name is correct." 

| Method                | Transcript                                      | Latency  | Accuracy  |
|-----------------------|------------------------------------------------|---------|-----------|
| **Streaming**         | Yes. The details for the deal name is correct. | real-time | 100%  |
| **AssemblyAI**        | Yes. The details for the deal name is correct. | 6.09s   | 100%  |
| **Faster Whisper Small**  | Yes, the details for the deal name is correct. | 0.28s   | 100%  |
| **Faster Whisper Medium** | Yes, the details for the deal name is correct. | 0.62s   | 100%  |

******

## **Sample Input Output for Large Language Models**
### Sample Input
```python

``` 
### Sample Prompt to LLM
```python

```
### Sample Output
```python

```
******


## **Sample Input Output for Embeddings Models**
### Sample Input
```python
"Can you make me a quote for 10 Starter Licences?"
``` 
### Sample Output
```python
[
  0.0123, -0.0456, 0.0789, 0.1023, -0.0567, ..., 0.0345
]
```
******


## **Sample Step-by-Step Process for Vector Database Training**
### Sample Products Table Data
| id  | name            | category | description                | price | tags                |
| --- | --------------- | -------- | -------------------------- | ----- | ------------------- |
| 1   | Starter License | Software | Basic license for one user | 100.0 | starter, basic      |
| 2   | Pro License     | Software | Advanced license for teams | 250.0 | pro, advanced, team |
| 3   | Enterprise Plan | Software | Full suite for enterprises | 500.0 | enterprise, full    |

### Consolidated Columns Result
Before generating embeddings, relevant text-based columns are combined into a single field:
| id  | consolidated_text                                                                                            |
| --- | ------------------------------------------------------------------------------------------------------------ |
| 1   | "Name: Starter License, Category: Software, Description: Basic license for one user, Tags: starter, basic"   |
| 2   | "Name: Pro License, Category: Software, Description: Advanced license for teams, Tags: pro, advanced, team"  |
| 3   | "Name: Enterprise Plan, Category: Software, Description: Full suite for enterprises, Tags: enterprise, full" |

### Sample Embeddings Table Data
After generating embeddings, the results are stored in a **PostgreSQL** table with a `vector` column (assuming **pgvector** is used):
| id  | product_id | embedding (vector representation) |
| --- | ---------- | --------------------------------- |
| 1   | 1          | `[0.0123, -0.0456, 0.0789, ...]`  |
| 2   | 2          | `[0.0987, -0.0123, 0.0567, ...]`  |
| 3   | 3          | `[0.0234, 0.0781, -0.0452, ...]`  |

******