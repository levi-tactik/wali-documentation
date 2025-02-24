

## **Comparison and Results - Speech-to-Text**
## Accuracy and Latency
The comparison involved conducting three separate recordings, each designed to represent different aspects of a conversation one might have with WALI. These recordings were then transcribed using multiple speech-to-text technologies to assess their performance.  

For **streaming transcription**, the **final transcript** was tracked in real time as the recording progressed, ensuring that the transcription reflected the spoken words without post-processing.  

For **Faster Whisper**, two different model sizes—**small** and **medium**—were implemented. These models transcribed the recordings after completion, allowing for a comparison of accuracy and processing speed between them.  

To ensure consistency across all transcription methods, **all audio files were recorded at a fixed sample rate of 48 kHz**. A sample rate of 48 kHz means that the audio signal was captured **48,000 times per second**, providing a higher resolution of sound, which can improve transcription accuracy by preserving more details of the speech.  

For **streaming transcription**, a **FRAMES_PER_BUFFER** value of **4096** was used. This parameter determines the number of audio frames processed at a time before being sent for transcription. A larger buffer size like 4096 helps balance real-time performance with stability by reducing the number of small, frequent transmissions, which can be computationally expensive, while still allowing for near-instantaneous transcription.  

Both **Faster Whisper** models ran on **CUDA**, which enabled GPU acceleration for faster processing. This allowed the models to handle speech-to-text conversion more efficiently than a CPU, improving speed while maintaining accuracy.  

For offline models, only transcription times were measured. In actual usage, these models require additional time to first save the recording before transcription, which is not reflected in the recorded latency times.

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

## Cost-Effectiveness
| Technology               | Cost Estimation         |
|--------------------------|------------------------|
| AssemblyAI Transcription | Best: $0.37/hr  <br> Nano: $0.12/hr |
| Faster Whisper           | Open source, N/A      |
| Assembly AI Streaming    | $0.47/hr              |

## Language Support
| Technology               | No. of Supported Languages        |
|--------------------------|------------------------|
| AssemblyAI Transcription | 99+|
| Faster Whisper           | 99+      |
| Assembly AI Streaming    | -              |

******

## **Sample Input Output for Large Language Models**
### Sample Input
```python
"I want to know how to bake a cake"
``` 
### Sample Prompt to LLM
```python
"""
    You are an AI assistant analyzing messages to determine if they are about making a sales quote. Given an input message which is: 'I want to know how to bake a cake', evaluate whether it involves a product being quoted along with other supporting details.  

    to provide context, this was the previous question to the user: 'Lets set up your quote, give me some details.'
    it is possible that the input message answers or contributes given the last question asked by you

    Respond with:  
    - **1** if the message talks about creating a sales quote, can be somewhat vague, can also include relevant details such as products and amount. can also include the user mentioning the state of their quote, such as them being satisfied, or not yet satisfied with the current quote details, if the input message is also along the lines of 'my quote is good and final', give it this value as well
    - **0** if the message does not mention a product or is unrelated to creating a sales quote.  
    - **0** if the message is unclear or lacks enough information to determine its intent.  

    Reply in this JSON format:  
    "mssg_eval": <1/0>
    "reason": <provide a short reason as to why you gave that evaluation>
"""
```
### Sample Output
```python
{
    "mssg_eval": 0,
    "reason": "The message is about baking a cake, which is unrelated to creating a sales quote or providing details about a product or amount."
}
```
******

## **Comparison and Results - Large Language Models**
## Accuracy, Contextual Awareness, and Latency
In generating these comparisons, the same configurations were applied to both models to ensure consistency. This included maintaining default hyperparameters such as temperature, top-p, and max tokens, thereby minimizing variability introduced by parameter adjustments. Additionally, both models were given the same prompt, which, while not ideal, allowed for a controlled evaluation of their ability to extract information accurately within a fixed structure.

The prompts primarily centered on the quoting process, assessing whether the models could correctly extract the necessary data while adhering to a predefined format. The provided context, including relevant product details and the most recent questions previously asked by WALI, was also kept similar to ensure a fair comparison.
### Scenario 1 - "Make me a quote for 5 enterprise licenses"
| Aspect         | GPT-4o                                                           | GPT-4o-mini                                                   |
| -------------- | ---------------------------------------------------------------- | ------------------------------------------------------------- |
| `latency`      | 2.75                                                             | 1.84                                                          |
| `product_id`   | "649a2c5b-9391-4816-9796-8d4aa686aea9"                           | "649a2c5b-9391-4816-9796-8d4aa686aea9"                         |
| `product_name` | "Enterprise"                                                    | "Enterprise"                                                   |
| `quantity`     | 5                                                                | 5                                                              |

### Scenario 2 - "Make me a quote for 5 enterprise licenses and 3 Starter licenses"
| Aspect           | GPT-4o                                                                                              | GPT-4o-mini                                                                                          |
| ---------------- | --------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| `latency`        | 5.07                                                                                                | 3.34                                                                                                  |
| `product_id_1`   | "649a2c5b-9391-4816-9796-8d4aa686aea9"                                                              | "649a2c5b-9391-4816-9796-8d4aa686aea9"                                                                |
| `product_name_1` | "Enterprise"                                                                                        | "Enterprise"                                                                                          |
| `quantity_1`     | 5                                                                                                   | 5                                                                                                     |
| `product_id_2`   | "c0670345-f13e-4b2a-ac53-dd5494c7a21d"                                                              | "c0670345-f13e-4b2a-ac53-dd5494c7a21d"                                                                |
| `product_name_2` | "Starter"                                                                                           | "Starter"                                                                                             |
| `quantity_2`     | 3                                                                                                   | 3                                                                                                     |

### Scenario 3 - With the quote currently having 5 enterprise licenses, Message: "Can you add an additional 5 platinum support licenses"
| Aspect           | GPT-4o                                                                                              | GPT-4o-mini                                                                                          |
| ---------------- | --------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| `latency`        | 3.34                                                                                                | 3.29                                                                                                  |
| `product_id_1`   | "649a2c5b-9391-4816-9796-8d4aa686aea9"                                                              | "649a2c5b-9391-4816-9796-8d4aa686aea9"                                                                |
| `product_name_1` | "Enterprise"                                                                                        | "Enterprise"                                                                                          |
| `quantity_1`     | 5                                                                                                   | 5                                                                                                     |
| `product_id_2`   | "305bfe19-bdd5-4333-84ae-2a720f43ad98"                                                              | "305bfe19-bdd5-4333-84ae-2a720f43ad98"                                                                |
| `product_name_2` | "Platinum Support"                                                                                  | "Platinum Support"                                                                                   |
| `quantity_2`     | 5                                                                                                   | 5                                                                                                     |

### Scenario 4 - With the quote currently having 5 enterprise licenses, Message: "Can you replace the 5 enterprise licenses with 5 platinum support licenses"
| Aspect           | GPT-4o                                                                                              | GPT-4o-mini                                                                                          |
| ---------------- | --------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| `latency`        | 2.53                                                                                                | 6.36                                                                                                  |
| `product_id`     | "305bfe19-bdd5-4333-84ae-2a720f43ad98"                                                              | "305bfe19-bdd5-4333-84ae-2a720f43ad98"                                                                |
| `product_name`   | "Platinum Support"                                                                                  | "Platinum Support"                                                                                   |
| `quantity`       | 5                                                                                                   | 5                                                                                                     |

### Scenario 5 - With the quote currently having 5 enterprise licenses, Message: "Can you add an additional 5 enterprise licenses to the quote?"
| Aspect           | GPT-4o                                                                                              | GPT-4o-mini                                                                                          |
| ---------------- | --------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| `latency`        | 3.02                                                                                                | 2.97                                                                                                  |
| `product_id`     | "649a2c5b-9391-4816-9796-8d4aa686aea9"                                                              | "649a2c5b-9391-4816-9796-8d4aa686aea9"                                                                |
| `product_name`   | "Enterprise"                                                                                        | "Enterprise"                                                                                          |
| `quantity`       | 10                                                                                                  | 10                                                                                                    |

## Cost-Effectiveness
| Technology          | Cost Estimation                                      |
|--------------------|------------------------------------------------------|
| OpenAI - GPT-4o    | Input - $2.50/1M tokens  |
|                    | Cached input - $1.25/1M tokens  |
|                    | Output - $10.00/1M tokens  |
| OpenAI - GPT-4o-mini | Input - $0.150/1M tokens  |
|                    | Cached Input - $0.075/1M tokens  |
|                    | Output - $0.600/1M tokens  |


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

## **Comparison and Results - Embeddings Model**
## Accuracy, Contextual Awareness, Robustness to Typos and Errors, and Latency
In this comparison, the same approach was applied across all models to ensure consistency in evaluation. Cosine distance was used to measure similarity between the user query and stored product embeddings, allowing for a direct comparison of retrieval effectiveness.

The dataset for product embeddings was relatively small, consisting of only five products. As a result, the retrieval process was limited to returning the two most relevant products per query. This constraint affected the diversity of results but ensured that the models focused on the highest-ranked matches.

Each model's ability to retrieve relevant products was assessed based on predefined prompts related to the quoting process. Additionally, contextual awareness was evaluated by providing prompts that referenced multiple products in a single request. The models' robustness to typos was also considered by including a prompt containing an intentional typo.

The results in the table represent the products that each model deemed most relevant based on the given message, highlighting differences in retrieval accuracy, contextual understanding, and error tolerance.

| **Prompt**                                     | **OpenAI Results (Latency: s)**                                | **MiniLM Results (Latency: s)**                              | **Word2Vec (w2v) Results (Latency: s)**                        |
|-----------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| **"Make me a quote for ten starter licenses"**  | Starter, Enterprise *(Latency: 1.13s)*                        | Starter, Business *(Latency: 0.05s)*                         | Starter, Platinum Support *(Latency: 0.00s)*                  |
| **"Make a quote for both Starter Licenses and Platinum Support"** | Platinum Support, Gold Support *(Latency: 0.41s)*            | Platinum Support, Starter *(Latency: 0.01s)*                 | Platinum Support, Gold Support *(Latency: 0.00s)*             |
| **"Starter"**                                   | Starter, Platinum Support *(Latency: 0.61s)*                  | Starter, Enterprise *(Latency: 0.05s)*                        | Starter, Platinum Support *(Latency: 0.00s)*                  |
| **"Entrepise"**                                 | Enterprise, Business *(Latency: 0.43s)*                        | Business, Starter *(Latency: 0.02s)*                          | *(No results)* *(Latency: 0.00s)*                             |

## Cost-Effectiveness
| Technology                     | Cost Estimation      |
|--------------------------------|----------------------|
| OpenAI - text-embedding-ada-002 | $0.0001/1K tokens   |
| all-MiniLM-L6-v2               | Open source, N/A    |
| Word2Vec                       | Open source, N/A    |

******

## **Comparison and Results - Text-to-Speech**
To ensure consistency, each model was tested with the same set of prompts, covering various sentence lengths and complexities. Coqui TTS was executed with CUDA acceleration enabled, leveraging GPU processing to optimize inference speed.
## Latency
| Message | Google (s) | pyttsx3 (s) | Coqui (s) |
|---------|------------|------------|-----------|
| *I do not quite understand, can you please repeat?* | 3.05 | 4.11 | 7.90 |
| *Let's set up your quote, please provide me some details* | 3.66 | 4.43 | 9.09 |
| *Your quote is good and final, let's print your PDF* | 3.98 | 4.36 | 8.14 |
| *I have added 5 enterprise licenses to your quote, let me know if this is good and final* | 6.01 | 6.23 | 13.38 |


## Human-Like Speech, Voice Variety and Customization
| Technology                    | Human-Like Speech                                      | Voice Variety                                 | Customization                                      |
|--------------------------------|------------------------------------------------------|----------------------------------------------|--------------------------------------------------|
| **pyttsx3**                    | Robotic and less natural compared to neural TTS models | Limited to system voices (depends on OS)    | Minimal customization (pitch, rate, volume)    |
| **Coqui-TTS**                  | More natural and expressive, especially with fine-tuned models | Supports multiple voices, can train custom voices | High customization with fine-tuning and voice cloning |
| **Google Cloud Text-to-Speech** | High-quality, human-like neural voices               | Wide range of voices and languages          | Supports pitch, speed, SSML, and custom voice training |


## Cost-Effectiveness
| Technology               | Cost Estimation         |
|--------------------------|------------------------|
| pyttsx3 | Open source, N/A    |
|   Coqui-TTS         | Open source, N/A      |
| Google Cloud Text-to-Speech    | Standard voices - $0.000004 per character (US$4 per 1 million characters)            |

