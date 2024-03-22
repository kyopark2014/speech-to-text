# Speech을 Text로 변환하기

GenAI를 음성으로 제어하기 위해서는 Speech to text 기술이 필요합니다. 여기서는 [AWS Transcribe](https://aws.amazon.com/ko/transcribe/)을 이용하여 실시간으로 음성을 텍스트로 변환하는것을 설명합니다.

## Speech 변환 주요 함수

[sounddevice](https://pypi.org/project/sounddevice/)를 이용해 아래와 같이 audio stream에서 음성 데이터를 추출합니다.

```python
stream = sounddevice.RawInputStream(
  channels=1,
  samplerate=16000,
  callback=callback,
  blocksize=1024 * 2,
  dtype="int16",
)

with stream:
  while True:
    indata, status = await input_queue.get()
    yield indata, status
```

아래와 같이 사용할 region을 지정한 TranscribeStreamingClient을 이용하여 아래와 같이 텍스트 stream을 가져옵니다. 이때 변환할 언어('language_code')와 sampling rate, encoding 방식을 지정합니다. 

```python
from amazon_transcribe.client import TranscribeStreamingClient

async def basic_transcribe():
    # Setup up our client with our chosen AWS region
    client = TranscribeStreamingClient(region="ap-northeast-2")

    # Start transcription to generate our async stream
    stream = await client.start_stream_transcription(
        language_code="ko-KR",
        media_sample_rate_hz=16000,
        media_encoding="pcm",
    )

    # Instantiate our handler and start processing events
    handler = MyEventHandler(stream.output_stream)
    await asyncio.gather(write_chunks(stream), handler.handle_events())
````

```python
async def handle_transcript_event(self, transcript_event: TranscriptEvent):
  results = transcript_event.transcript.results
                       
    for result in results:       
      if result.is_partial == False:
        for alt in result.alternatives:
          print('----> ', alt.transcript)
          #msg = {
          #  "userId": userId,
          #  "query": alt.transcript,
          #  "state": "completed"
          #}
          #resp = requests.post(url, json=msg)
          #print('resp: ', resp)
      else:
        for alt in result.alternatives:
          print(alt.transcript)    
          #msg = {
          #  "userId": userId,
          #  "query": alt.transcript,
          #  "state": "proceeding"
          #}
          #resp = requests.post(url, json=msg)
```




## Voice Interpreter의 설치 및 실행

여기서 설명하는 Voice Interpreter는 [Amazon Transcribe Streaming SDK](https://github.com/awslabs/amazon-transcribe-streaming-sdk)을 참조하였습니다. Voice Interpreter는 음성으로부터 Text를 추출합니다. 

먼저 아래와 같이 github을 다운로드하고 interpreter 폴더로 이동합니다. 

```text
git clone https://github.com/kyopark2014/speech-to-text
cd interpreter
```

실행에 필요한 필요한 라이브러리를 설치합니다. 여기서 설치하는 프로그램 패키지는 [amazon-transcribe](https://pypi.org/project/amazon-transcribe/)와 [sounddevice](https://pypi.org/project/sounddevice/)로서 requirements를 아래와 같이 설치합니다.

```text
pip install -r requirements.txt
```

이후 아래와 같이 실행합니다.

```text
python mic_main.py
```

실제 실행 결과는 아래와 같습니다.

```text
# python mic_main.py 
url:  https://dxt1m1ae24b28.cloudfront.net/redis
userId:  robot
않냐?
안녕하세요
안녕하세요
---->  안녕하세요
```
