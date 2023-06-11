# AIfinal
class SimpleChatBot:

    def load_data(self, filepath):
        data = pd.read_csv(filepath)
        questions = data['Q'].tolist()  # 질문열만 뽑아 파이썬 리스트로 저장
        answers = data['A'].tolist()   # 답변열만 뽑아 파이썬 리스트로 저장
        return questions, answers

    def calc_distance(a, b):
        ''' 레벤슈타인 거리 계산하기 '''
        if a == b: return 0 # 같으면 0을 반환
        a_len = len(a) # a 길이
        b_len = len(b) # b 길이
        if a == "": return b_len
        if b == "": return a_len
    # 2차원 표 (a_len+1, b_len+1) 준비하기 --- (※1)
        matrix = [[] for i in range(a_len+1)] # 리스트 컴프리헨션을 사용하여 1차원 초기화
        for i in range(a_len+1): # 0으로 초기화
            matrix[i] = [0 for j in range(b_len+1)]  # 리스트 컴프리헨션을 사용하여 2차원 초기화
    # 0일 때 초깃값을 설정
        for i in range(a_len+1):
            matrix[i][0] = i
        for j in range(b_len+1):
            matrix[0][j] = j
    # 표 채우기 --- (※2)
        for i in range(1, a_len+1):
            ac = a[i-1]
            for j in range(1, b_len+1):
                bc = b[j-1] 
                cost = 0 if (ac == bc) else 1  #  파이썬 조건 표현식
                matrix[i][j] = min([
                    matrix[i-1][j] + 1,     # 문자 제거: 위쪽에서 +1
                    matrix[i][j-1] + 1,     # 문자 삽입: 왼쪽 수에서 +1   
                    matrix[i-1][j-1] + cost # 문자 변경: 대각선에서 +1, 문자가 동일하면 대각선 숫자 복사
                ])
        return matrix[a_len][b_len]
    
    def find_best_answer(self, input_sentence):
        for n in self.questions: #반복문으로 레벤슈타인 최소 거리 인덱스 구하기
            r=[]  #빈 리스트 r
            r.append(calc_distance(input_sentence,self.questions[n])) #레벤슈타인 거리 리스트화
        best_match_index=r.index(min(r)) #레벤슈타인 최소 거리 인덱스
        return self.answers[best_match_index] #답 도출
        
        
    # CSV 파일 경로를 지정
filepath = 'ChatbotData.csv'

    # 간단한 챗봇 인스턴스를 생성합니다.
chatbot = SimpleChatBot(filepath)

    # '종료'라는 단어가 입력될 때까지 챗봇과의 대화를 반복합니다.
while True:
    input_sentence = input('You: ')
    if input_sentence.lower() == '종료':
        break
    response = chatbot.find_best_answer(input_sentence)
    print('Chatbot:', response)
    
