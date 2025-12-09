# YouTube Data Collector MCP Server

이 레포지토리는 **FastMCP**를 기반으로 구축된 YouTube 데이터 수집용 MCP(Model Context Protocol) 서버입니다.

Claude, ChatGPT 등의 LLM을 YouTube API와 연동하여 **자막 추출, 영상 검색, 채널 분석, 댓글 수집** 등의 작업을 수행할 수 있도록 4가지 핵심 도구(Tool)를 제공합니다.

## 주요 기능

이 MCP 서버는 다음과 같은 기능을 제공합니다.

1. **자막(Transcript) 추출**: YouTube 영상의 자막을 가져옵니다 (한국어/영어 지원).
2. **영상 검색**: 키워드로 영상을 검색하고 조회수, 좋아요 수 등 세부 정보를 확인합니다.
3. **채널 정보 분석**: 특정 영상의 채널 정보를 파악하고, 해당 채널의 최근 업로드 목록을 가져옵니다.
4. **댓글 수집**: 영상의 댓글을 관련성 또는 시간순으로 수집하여 여론을 파악합니다.

## 기술 스택 (Tech Stack)

- **Python 3.10+**
- **FastMCP**: MCP 서버 구축 프레임워크
- **YouTube Data API v3**: 영상 메타데이터 및 댓글 검색
- **youtube-transcript-api**: 비공식 자막 추출 라이브러리

## 환경 변수 설정

이 서버를 실행하기 위해서는 Google Cloud Platform에서 발급받은 YouTube Data API 키가 필요합니다. FastMCP Cloud의 **Secrets** 설정에 다음 변수를 추가해주세요.

| 변수명 | 설명 |
| :--- | :--- |
| `YOUTUBE_API_KEY` | **(필수)** Google Cloud Console에서 발급받은 YouTube Data API v3 Key |

## 제공 도구 상세

### 1. `get_youtube_transcript`
유튜브 영상의 자막 텍스트를 추출합니다. 내용을 요약하거나 분석할 때 유용합니다.

- **Arguments:**
  - `url` (str): 분석할 유튜브 영상 URL
  - `languages` (list[str]): 우선순위 언어 코드 (기본값: `['ko', 'en']`)

### 2. `search_youtube_videos`
키워드를 사용하여 유튜브 영상을 검색합니다.

- **Arguments:**
  - `query` (str): 검색어
  - `order` (str): 정렬 기준 (`relevance`, `date`, `viewCount`, `rating`, `title`, `videoCount` / 기본값: `relevance`)
  - `max_results` (int): 반환할 영상 개수 (기본값: 5)

### 3. `get_channel_info`
특정 영상이 속한 채널의 정보(구독자 수, 총 조회수)와 해당 채널의 최신/인기 영상을 조회합니다.

- **Arguments:**
  - `video_url` (str): 대상 유튜브 영상 URL (채널 식별용)
  - `order` (str): 영상 목록 정렬 기준 (`date`, `viewCount`, `rating` / 기본값: `date`)
  - `max_results` (int): 가져올 채널 영상 개수 (기본값: 5)

### 4. `get_youtube_comments`
영상의 댓글 반응을 수집합니다.

- **Arguments:**
  - `video_url` (str): 대상 유튜브 영상 URL
  - `order` (str): 정렬 기준 (`relevance`: 인기순, `time`: 최신순 / 기본값: `relevance`)
  - `max_results` (int): 수집할 댓글 수 (기본값: 10)

## 배포 및 실행

이 프로젝트는 **FastMCP Cloud**에 최적화되어 있습니다.

1. 이 레포지토리를 GitHub에 푸시합니다.
2. FastMCP Cloud 대시보드에서 레포지토리를 연결합니다.
3. `YOUTUBE_API_KEY` 환경 변수를 설정합니다.
4. 배포가 완료되면 Claude Desktop 또는 지원되는 클라이언트에서 해당 서버를 사용할 수 있습니다.

---

# YouTube Data Collector MCP Server

This repository is a YouTube data collection MCP (Model Context Protocol) server built on **FastMCP**.

It provides 4 core tools that allow LLMs like Claude and ChatGPT to interact with the YouTube API for tasks such as **transcript extraction, video search, channel analysis, and comment collection**.

## Key Features

This MCP server provides the following functionalities:

1. **Transcript Extraction**: Retrieves subtitles from YouTube videos (Supports Korean/English).
2. **Video Search**: Searches for videos by keyword and retrieves details like view counts and likes.
3. **Channel Analysis**: Identifies channel information from a specific video and fetches the channel's recent uploads.
4. **Comment Collection**: Collects video comments sorted by relevance or time to analyze public opinion.

## 🛠️ Tech Stack

- **Python 3.10+**
- **FastMCP**: Framework for building MCP servers
- **YouTube Data API v3**: For retrieving video metadata and comments
- **youtube-transcript-api**: Unofficial library for extracting transcripts

## Environment Variables

To run this server, a YouTube Data API key from Google Cloud Platform is required. Please add the following variable to your **Secrets** configuration in FastMCP Cloud.

| Variable Name | Description |
| :--- | :--- |
| `YOUTUBE_API_KEY` | **(Required)** YouTube Data API v3 Key issued from Google Cloud Console |

## Available Tools

### 1. `get_youtube_transcript`
Extracts transcript text from a YouTube video. Useful for content summarization or analysis.

- **Arguments:**
  - `url` (str): The URL of the YouTube video to analyze.
  - `languages` (list[str]): Priority list of language codes (default: `['ko', 'en']`).

### 2. `search_youtube_videos`
Searches for YouTube videos using specific keywords.

- **Arguments:**
  - `query` (str): Search keyword.
  - `order` (str): Sort order (`relevance`, `date`, `viewCount`, `rating`, `title`, `videoCount` / default: `relevance`).
  - `max_results` (int): Number of videos to return (default: 5).

### 3. `get_channel_info`
Retrieves information (subscribers, total views) for the channel associated with a specific video, along with its recent/popular videos.

- **Arguments:**
  - `video_url` (str): The URL of a video from the target channel (used to identify the channel).
  - `order` (str): Sort order for the video list (`date`, `viewCount`, `rating` / default: `date`).
  - `max_results` (int): Number of channel videos to retrieve (default: 5).

### 4. `get_youtube_comments`
Collects comments and reactions from a specific video.

- **Arguments:**
  - `video_url` (str): The URL of the target YouTube video.
  - `order` (str): Sort order (`relevance`: top rated, `time`: newest / default: `relevance`).
  - `max_results` (int): Number of comments to collect (default: 10).

## Deployment & Usage

This project is optimized for **FastMCP Cloud**.

1. Push this repository to GitHub.
2. Connect the repository in the FastMCP Cloud dashboard.
3. Set the `YOUTUBE_API_KEY` environment variable.
4. Once deployed, the server is available for use in Claude Desktop or other supported clients.
