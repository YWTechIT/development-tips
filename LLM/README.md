## 📍 ChatGPT Prompt Engineering for Developers 강의를 보고..

언젠가 tech-news 채널에서 prompt engineering을 다룬 글을 본 적이 있었다. 거기서 개발자들이 ChatGPT를 더 잘 활용하기 위해 이 <a href='https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/'>강의</a>를 보는 것이 도움된다는 내용을 까맣게 잊고 있다가 추석 연휴 때 생각나서 부랴부랴 강의를 봤다. 사이트는 DeepLearning.AI이고, Andrew Ng, Isa Fulford님이 진행하였다. 강의를 들으며 노션에 적었던 내용을 조금 적었다.

### Two Types of Large Language Models (LLMs)
LLM의 개발에는 두 가지의 유형으로 나뉘는데, Base LLM과 Instruction Tuned LLM이다. 

Base LLM은 텍스트 training data를 기반으로 다음 단어를 예측하도록 훈련되었고, 인터넷과 여러 출처 기반으로 한 많은 양의 데이터를 통해 훈련해 다음에 나올 가능성 가장 높은 단어가 무엇인지 파악한다. 예를 들면, 옛날 옛적이 어느 마을에~까지 prompt에 입력하면 문맥에 올바른 답변을 예측할 가능성이 높다. 하지만 파리의 수도가 어딘가요?와 같은 질문은 파리의 국기는 어떻게 생겼나요? 파리의 인구는 얼마인가요?와 같은 신문 기사에서 볼 법한 답변들이 나올 가능성이 높다. (인터넷의 기사는 프랑스에 대한 퀴즈 질문의 집합일 수 있기 때문에..)

Instruction Tuned LLM은 많은 LLM 연구와 실험이 진행된 지시사항에 맞춰 훈련된 LLM인 instruction-tuned LLM은 지시사항을 따르도록 훈련되었다. 대량의 텍스트 데이터로 훈련된 기본 LLM에서 시작하여, 지시사항의 입력과 출력 값을 활용해 더욱 fine-tunning 하고, 그 후에는 종종 RLHF라는 기법을 통해 더욱 세밀하게 fine-tunning 하며, 시스템에 도움이 되고 지시사항을 따르는 능력을 향상한다. 따라서 도움이 되고, 정직하며, 혐오 발언이 없는 텍스트에서 훈련되었기 때문에 기본 LLM에 비해 편견이나 혐오 발언 등의 문제가 될 수 있는 텍스트를 출력하는 확률이 낮다. 따라서 현업에서는 instruction-tuned LLM를 쓰는 추세이다. 인터넷에서 찾을 수 있는 일부 최선의 방법들은 기본 LLM에 더 적합할 수 있지만, 오늘날 실용적인 부분에서 instruction-tuned LLM을 더 권장하는데, 이는 사용하기 쉽고 openAI와 다른 LLM 회사들의 모델이 안전하고 정교해지고 있기 때문이다.

### Guidelines
ChatGPT가 질문에 대한 원하는 대답을 얻기 위해 강조하는 2가지 원칙이 있다. 첫 번째는 명확하고 구체적인 지시를 작성하는 것이고, 두 번째는 모델에게 생각할 시간을 주는 것이다. 

Principle 1. Write clear and specific instructions
1. Use delimeters: 구분자(delimeter)를 사용하여 입력의 구분된 부분을 명확하게 표시하여 prompt injection을 피할 수 있다. 
2. Ask for structured output: 모델 출력의 구문 분석을 더 쉽게 하기 위해, HTML이나 JSON과 같은 구조화된 출력을 요청한다.
3. Check whether conditions are satisfied: 작업이 조건에 부합하지 않는 가정을 한다면, 모델에게 이러한 가정을 확인하도록 지시한다.
4. Few-shot prompting: 원하는 작업의 성공적인 실행 예시를 제공하는 것. 모델에게 실제로 원하는 작업을 수행하도록 요청하기 전에 이루어진다.

Principle 2. Give the model time to think: 모델이 결론을 서두르다가 잘못된 결론을 내리는 추론 오류를 범한다면, 모델이 최종 답변을 제공하기 전에, 질의를 재구성하여 연관된 추론의 chain이나 series를 요청해야 한다. 다시 말해, 모델에게 짧은 시간이나 적은 단어로 너무 복잡한 작업을 주면, 모델은 잘못될 추측을 하게 될 가능성이 높다.
1. Specify the steps to complete a task
2. Instruct the model to work out its own solution before rushing to a conclusion

### Iterative
1. 가끔 사용자가 최대 n자 이내로 결과를 출력하라는 prompt를 보는데 이는 추천하지 않는다. LLM은 텍스트를 해석하는 방식을 토크나이저를 사용하기 때문이다. 토크나이저는 문자를 세는 데 그다지 능하지 않기 때문에 보통의 LLM은 비슷하게 맞추지 못한다.(강의에서 ChatGPT는 n자를 거의 맞춰서 결과를 출력했다.)
2. 한 번의 프롬프트로 완벽한 답변을 얻으려 하지 말고, 반복적으로 지시사항을 어떻게 명확하게 할지 생각하는 과정을 거쳐야 한다. 유능한 프롬프트 엔지니어의 핵심은 완벽한 프롬프트를 알고 있는 것이 아니라, 여러분의 애플리케이션의 효과적인 프롬프트를 개발하는 좋은 과정을 알고 있는 것으로 생각한다.

이외에도 Model Limitations, Inferring, Transforming, Expanding 등에 관한 설명을 들었다. 물론 강의에서 설명해주는 내용이 개발을 하는데 전부 사용되진 않는다. 하지만 가려운 곳을 긁어주는 내용도 있었다. 예를 들면, ChatGPT에게 궁금한 점을 물어보면 그럴듯하지만 실제로 사실이 아닌 것을 만들어내는 환영(Hallucination)으로 답변하는 경우가 종종 있다. 강의에서는 이런 경우 관련된 인용구를 찾게 하여 질문에 답하도록 요청하고, 그 답변을 원문에서 추적하는 것이 대부분 경우 매우 유용하다고 나와 있다. 인용구라면 공식문서, issues 등에서 찾아서 답변하도록 유도하라는 얘기인가 싶었고, 한편으로는 하나하나씩 ChatGPT에게 유도하기보단 차라리 내가 공식문서를 읽는 게 더 빠를 수도 있지않을까?란 생각이 들었다.

여튼 2시간 남짓한 강의를 들으며 LLM의 유형과 prompt engineering의 기본적인 방법을 배웠다. ChatGPT를 자주 사용하는 개발자나 prompt engineer를 배우고 싶은 입문자에게는 추천할만한 강의였다.