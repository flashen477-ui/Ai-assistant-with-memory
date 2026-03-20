import os
import json
import random

USER_FILE="user.json"
NOTES_FILE="notes.txt"

MOTIVATIONAL_PHRASES = [
    "Ти зможеш більше, ніж думаєш!",
    "Кожен день - це новий шанс стати кращим!",
    "Віра в себе - це перший крок до успіху!",
    "Навіть маленький крок вперед - це прогрес!",
    "Ти сильніший, ніж твої сумніви!"
]

JOKES = [
    "Чому програмісти не люблять природу? Бо там багато багів!",
    "Як називається ведмідь без зубів? М'який ведмідь!",
    "Чому слон не грає в шахи? Бо не вміє ходити конем!",
    "Що каже один нуль іншому? Без тебе я ніщо!"
]

MOODS = ["щасливий", "енергійний", "креативний", "спокійний", "задумливий"]

print("Асистент 4.0: Завантаження модулів пам'яті...")

if os.path.exists(USER_FILE):
    with open(USER_FILE,"r",encoding="utf-8") as file:
        user_data=json.load(file)
        user_name=user_data["name"]   
    print(f"Асистент: З поверненням {user_name}! Радий тебе бачити.")
else:
    print("Асистент: Привіт, здається ми не знайомі!")
    user_name=input("Як тебе звати? ")
    
    user_data={"name":user_name}
    with open(USER_FILE,"w",encoding="utf-8") as file:
        json.dump(user_data,file,ensure_ascii=False,indent=4)
    print(f"Асистент: Приємно познайомитись, {user_name}! Я запам'ятав твоє ім'я.")

while True:
    user_input=input(f"\n{user_name}: ").lower()
    
    if user_input=="вихід":
        print("Асистент: До побачення!")
        break
    
    elif "мотивація" in user_input or "підбадьор" in user_input:
        print(f"Асистент: {random.choice(MOTIVATIONAL_PHRASES)}")
    
    elif "жарт" in user_input:
        print(f"Асистент: {random.choice(JOKES)}")
    
    elif "настрій" in user_input:
        print(f"Асистент: Обери настрій: {', '.join(MOODS)}")
        mood_input = input(f"{user_name}: ").lower()
        if mood_input in MOODS:
            print(f"Асистент: Я запам'ятав, що ти зараз {mood_input}")
            if "mood_history" not in user_data:
                user_data["mood_history"] = []
            user_data["mood_history"].append(mood_input)
            with open(USER_FILE,"w",encoding="utf-8") as file:
                json.dump(user_data,file,ensure_ascii=False,indent=4)
        else:
            print("Асистент: Вибери настрій зі списку")
    
    elif "запиши" in user_input or "нотатки" in user_input and not "прочитай" in user_input:
        note_text=input("Асистент: Що саме записати: ")
        with open(NOTES_FILE, "a",encoding="utf-8") as file:
            file.write(note_text+"\n")
        print("Асистент: Нотатку збережено.")
    
    elif "прочитай нотатки" in user_input:
        if os.path.exists(NOTES_FILE):
            print("Асистент: Відкриваю твої нотатки...")
            print("-"*20)            
            with open(NOTES_FILE,"r",encoding="utf-8") as file:
                notes=file.read()
                print(notes)
            print("-"*20)
        else:
            print("Асистент: В тебе немає нотаток")
    
    else:
        print("Асистент: Я можу писати та читати нотатки, давати мотивацію, жартувати та слідкувати за настроєм")
