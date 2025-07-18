# 🧭 메모리네비 (MemoryNavi)
> **치매 관련 정보 제공 AI 챗봇**
> 메모리네비는 고령자를 위한 치매 제도 및 복지 정보 탐색을 돕는 문서 기반 AI 챗봇입니다.      
> PDF 문서를 벡터화하여 최신 내용을 자동 분석하고, RAG 기술 기반으로 질문 의도에 맞는 신뢰도 높은 맞춤형 답변을 제공합니다.    
> CSS를 사용하여 직관적이고 큰 글씨 UI로 어르신이 쉽게 이용할 수 있도록 설계되었습니다.    
> Streamlit 기반 웹 인터페이스로 배포하였습니다.

---

## 주요 기능
- PDF 문서 기반 RAG(Retrieval-Augmented Generation) 시스템
- 치매 관련 국가 제도, 복지, 의료 정보 자동 응답
- OpenAI GPT-4o-mini 기반 대화형 AI
- 사용자 입력/AI 응답 UI 커스터마이징 (고령자 친화형 폰트 및 구성)
- Streamlit 기반 웹 인터페이스
- 대화 히스토리 기억 및 반영

---

## 시스템 아키텍처

| 구성 요소       | 설명 |
|----------------|------|
| **UI**         | Streamlit 기반 채팅 인터페이스 |
| **문서 로더**  | `PyPDFLoader` (PDF → 텍스트 분할) |
| **텍스트 분할기** | `RecursiveCharacterTextSplitter` (chunk_size=1000, overlap=200) |
| **임베딩 모델** | OpenAI `text-embedding-3-large` |
| **벡터 DB**    | FAISS (로컬 저장 및 로딩 지원) |
| **LLM**        | OpenAI `gpt-4o-mini` |
| **Retriever**  | 문서 검색 + 대화 이력 기반 `create_history_aware_retriever()` |
| **QA Prompt**  | 문서 기반 응답 생성 프롬프트 (`ChatPromptTemplate`) |
| **Chain 구성** | `create_retrieval_chain()` + `RunnableWithMessageHistory` |

---

## RAG 처리 흐름도

<pre> 
[PDF 문서 입력]    
        │    
        ▼    
[문서 로더]    
(PDF → 텍스트 분할)    
→ PyPDFLoader    
→ RecursiveCharacterTextSplitter    
        │    
        ▼    
[텍스트 임베딩]    
→ text-embedding-3-large    
        │    
        ▼    
[벡터 저장]    
→ FAISS 벡터 DB    
        │
        ▼
[질문 입력]
        │
        ▼
[질문 리포맷]
→ History-aware Retriever
        │
        ▼
[유사 문서 검색]
→ Top 2 검색
        │
        ▼
[문서 기반 응답 생성]
→ GPT-4o-mini + QA Prompt
        │
        ▼
[Streamlit UI 출력]
→ 사용자에게 응답 표시
→ (참고 문서 펼치기 가능)

</pre>

## 👴 페르소나: 관식 할아버지

| 구분       | 내용 |
|------------|------|
| **이름**    | 관식 할아버지 |
| **나이**    | 75세 |
| **거주지**  | 수도권 거주중 |
| **직업/경력** | 은퇴 후 ‘노인 일자리’ 근무중 |
| **가족 관계** | 독신 |
| **기술 활용** | 스마트폰으로 **카카오톡·유튜브** 정도만 사용<br>복잡한 앱·웹 검색은 어려움 |
| **성격**    | 은퇴 후에도 직접 생계활동을 할 만큼 자립적이지만 자녀가 없고 사회적 도움이 필요한 상황 |
| **목표**    | - 치매 등 돌발 상황이 와도 재산이 안전하게 관리되길 원함<br>- 남은 여생 동안 자식이나 돌봐줄 가족이 없기 때문에 사회적인 도움이 필요함<br>- 실버타운, 치매센터, 요양원 등 직접적으로 이용 가능한 자세한 정보가 필요함<br>- 금전관리·법률적인 정보를 얻어 실질적으로 대비할 수 있는 정보가 필요함 |

---

## 모두의연구소 랭체인톤 프로잭트입니다.
- 발표자료 : [LangchainThon_MemoryNevi.pdf](https://github.com/user-attachments/files/21020923/LangchainThon_MemoryNevi.pdf)
- 시연영상 : [MemoryNevi - 4일간 만든 생성형 AI 프로젝트 시연 | LangChainthon 2025] (https://youtu.be/jP0lnaHuH3c)


