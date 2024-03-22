# Speech을 Text로 변환하기

GenAI를 음성으로 제어하기 위해서는 Speech to text 기술이 필요합니다. 여기서는 [AWS Transcribe](https://aws.amazon.com/ko/transcribe/)을 이용하여 실시간으로 음성을 텍스트로 변환하는것을 설명합니다.

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
