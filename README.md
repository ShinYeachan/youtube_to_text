# **Youtube to Text 웹사이트**

Whisper AI 모델을 활용해 유투브 영상을 텍스트로 변환하는 웹사이트를 제작하였습니다.

## **시작하기 전에**

## ** 설치 및 환경 설정**

### ** webdriver_manager 및 selenium 설치**
- **selenium**: Selenium은 웹 브라우저에서 수행 된 테스트를 자동화하는 데 사용되는 오픈 소스 도구입니다
    ```bash
    pip install selenium
    ```
    
- **webdriver_manager**: 크롬 버전관리를 자동으로 관리하는 것을 도와주는 오픈 소스 도구입니다. 크롬 브라우저 버전이 업데이트 되더라도 그에 맞는 크롬 드라이버를 다운로드 해 줍니다.
    ```bash
    pip install webdriver-manager
    ```
  
### ** FFmpeg 설치**
- **FFmpeg**: 비디오, 오디오, 이미지 인코딩/디코딩 및 멀티플렉싱/디멀티플렉싱을 지원하는 멀티미디어 프레임워크입니다.
    ```bash
    # 리눅스
    sudo apt update && sudo apt install ffmpeg
    
    # MacOS
    brew install ffmpeg
    
    # 윈도우
    choco install ffmpeg
    ```

### ** Pytorch 설치**
- **Pytorch** : 파이토치는 딥러닝 프로젝트를 빌드(build)하는 데 도움을 주는 파이썬 프로그램용 라이브러리 입니다
- **참고**: 설치 명령어는 사용하고 있는 운영체제 및 환경에 따라 달라질 수 있습니다. 공식 웹사이트를 참고하세요.
- https://pytorch.kr/get-started/locally/
    ```bash
    # 이 명령어는 Linux의 PoPOS 및 Ubuntu 22.10에서 수행되었습니다.
    conda install pytorch torchvision torchaudio pytorch-cuda=11.7 -c pytorch -c nvidi 
    ```

### ** yt-whisper 설치**
- **yt-whisper** : yt-whisper는 인공지능 기술을 이용하여 음성 언어를 자동으로 텍스트로 변환해주는 소프트웨어인 WhisperAI를 이용해서 유투브 동영상을 자막으로 변환하게 해줍니다.
    
    ```bash
    pip install git+https://github.com/m1guelpf/yt-whisper.git  
    ```

### ** 기타 필요한 파이썬 패키지들 설치**

    pip install -r requirements.txt
    

| 라이브러리 | 설명                                                         |
|---|------------------------------------------------------------|
| chromedrivermanager | Chrome 웹 드라이버의 버전을 관리해주는 도구                                |
| ipython | 인터랙티브한 파이썬 셸을 제공하여 코드 실행을 간편하게 해주는 도구                      |
| openpyxl | 파이썬에서 Excel 파일을 읽고 쓰기 위한 라이브러리                             |
| pip | 파이썬 패키지를 설치 및 관리하는 도구                                      |
| pipdeptree | 파이썬 패키지의 종속성을 트리 형태로 보여주는 도구                               |
| PyAutoGUI | GUI 자동화를 위한 파이썬 모듈                                         |
| PySocks | 파이썬에서 SOCKS 프로토콜을 사용하기 위한 라이브러리                            |
| python-docx | 파이썬에서 MS Word 문서를 생성 및 수정하기 위한 라이브러리                       |
| pytube | 파이썬에서 YouTube 동영상을 다운로드하기 위한 라이브러리                         |
| selenium | 웹 브라우저를 자동화하여 웹 테스트 및 스크래핑을 위한 도구                          |
| setuptools | 파이썬 패키지를 만들고 배포하기 위한 도구                                    |
| st-file-browser | Streamlit 웹 애플리케이션에서 파일 브라우징을 지원하는 확장                      |
| streamlit | 빠르게 데이터 시각화 및 웹 애플리케이션을 만들 수 있는 도구                         |
| streamlit-folium | Streamlit에서 지도 시각화 라이브러리인 Folium을 사용하기 위한 확장               |
| tk | 파이썬에서 GUI 애플리케이션을 개발하기 위한 도구, 여기서는 폴더 선택기를 이용하기위해 사용되었습니다. |
| torchaudio | 오디오 처리를 위한 PyTorch 확장                                      |
| torchvision | 컴퓨터 비전 작업을 위한 PyTorch 확장                                   |
| wheel | 파이썬 배포 패키지 형식을 지원하는 도구                                     |

## ** 어플리케이션 기능**

### Helper 함수

- `setup_webdriver()`: Chrome WebDriver를 설정하고 반환합니다.
- `select_folder()`: 사용자에게 폴더를 선택하게 하는 UI를 보여줍니다.
- `create_directory_if_not_exists(directory_path)`: 주어진 경로에 디렉토리가 없는 경우 생성합니다.
- `sanitize_filename(filename)`: 파일 이름에서 유효하지 않은 문자를 제거합니다.
- `move_vtt_file_to_main_folder(directory, vtt_subfolder_path)`: .vtt 파일을 해당 서브 폴더에서 메인 폴더로 이동시킵니다.
- `rename_video_to_match_vtt(vtt_path)`: .vtt 파일과 동일한 이름으로 비디오를 이름을 변경합니다.

### 메인 함수

- `get_videos_data(driver, url)`: 주어진 URL의 YouTube 채널에서 비디오 데이터를 스크래핑합니다.
- `save_to_excel(channel_name, data, directory)`: 스크래핑한 데이터를 Excel 파일로 저장합니다.
- `download_videos(filename, directory)`: Excel 파일에 있는 YouTube 링크로부터 비디오를 다운로드합니다.
- `convert_to_vtt(filename, directory, model, task, language)`: 비디오를 .vtt 형식으로 변환합니다.

## ** 어플리케이션 실행**

### 1. 어플리케이션 시작하기
- 터미널을 열고 해당 폴더 경로로 이동합니다.
- 아래의 명령어를 터미널에 입력하여 어플리케이션을 실행합니다.
    ```bash
    streamlit run streamlit_youtube_to_mp4_and_subtitle.py
    ```

### 2. YouTube 채널 정보 입력하기
- YouTube 채널의 동영상 탭 URL을 입력란에 붙여넣습니다.

### 3. 저장 경로 설정하기
- `Select Folder` 버튼을 클릭하여 동영상 및 자막 파일을 저장할 위치를 선택합니다.

### 4. 변환 설정 선택하기
- **모델 선택**: Base, Small, Medium, Large 중 하나를 선택합니다. 
  -  Base < small < medium < large 모델 순으로 더 정확한 결과를 나타내나, 더 많은 시간과 성능을 필요로 합니다

- **작업 선택**: 
  - `transcribe`: 동영상의 오디오를 텍스트로 전사합니다. (영어 비디오면 `en` 선택, 한국어 비디오면 `ko` 선택)
  - `translate`: 지정된 언어로 텍스트를 번역합니다.

- **언어 선택**: 
  - `en`: 영어
  - `ko`: 한국어

### 5. 프로세스 시작하기
- `Fetch, Download and Convert` 버튼을 클릭하여 동영상 다운로드 및 자막 변환 프로세스를 시작합니다.

---

## **참조 링크**
[YouTube 참조 동영상](https://www.youtube.com/watch?v=cNLXzXyuzUs&t=1023s)




