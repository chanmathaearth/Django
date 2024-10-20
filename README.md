
### 1. **การตรวจสอบการติดตั้ง Python**

-   **ความสำคัญ**: ก่อนที่เราจะเริ่มต้นพัฒนา Django หรือสร้าง environment ต่าง ๆ เราจำเป็นต้องแน่ใจว่า Python ถูกติดตั้งในเครื่องแล้ว
-   **คำสั่งสำคัญ**:
    -   Windows: ใช้คำสั่ง `py --version` ใน Command Prompt เพื่อเช็คว่าเครื่องติดตั้ง Python หรือไม่ และอยู่ในเวอร์ชันใด
    -   MacOS: ใช้คำสั่ง `python --version` ใน Terminal
-   **ผลลัพธ์**: Python จะต้องเป็นเวอร์ชันที่ใช้งานได้เช่น 3.9.13 หรือสูงกว่า เพื่อให้รองรับ Django

### 2. **การตั้งค่า Virtual Environment (สิ่งแวดล้อมเสมือน)**

-   **ความสำคัญ**: Virtual environment คือสภาพแวดล้อมแยกที่ให้เราติดตั้ง libraries เฉพาะในโปรเจค เพื่อไม่ให้กระทบกับระบบหรือโปรเจคอื่น ๆ
-   **ขั้นตอน**:
    1.  **ติดตั้ง virtualenv**: ด้วยคำสั่ง `pip install virtualenv` จะติดตั้ง package ที่จำเป็นสำหรับการสร้าง virtual environment
    2.  **สร้าง virtual environment**:
        -   Windows: ใช้คำสั่ง `py -m venv myvenv`
        -   MacOS: ใช้คำสั่ง `python -m venv myvenv`
    3.  **การ activate virtual environment**:
        -   Windows: `myvenv\Scripts\activate.bat`
        -   MacOS: `source myvenv/bin/activate`
-   **ผลลัพธ์**: จะมีโฟลเดอร์ชื่อ `myvenv` เพิ่มขึ้นมาในโปรเจค ซึ่งเป็นที่จัดการ libraries และ dependencies ของโปรเจค

### 3. **การติดตั้ง Django**

-   **ความสำคัญ**: Django เป็น framework สำหรับการพัฒนาเว็บที่ช่วยให้เราสร้างแอปพลิเคชันได้รวดเร็วโดยใช้ Python
-   **ขั้นตอน**:
    -   ติดตั้ง Django ด้วยคำสั่ง `pip install django`
    -   ตรวจสอบว่า Django ติดตั้งสำเร็จหรือไม่ ด้วยคำสั่ง `python -m django --version`
-   **ผลลัพธ์**: ควรเห็นเวอร์ชัน Django เช่น 4.2.13 เป็นการยืนยันว่าการติดตั้งสำเร็จ

### 4. **การสร้างและการทำงานของ View ใน Django**

-   **ความสำคัญ**: View คือส่วนที่จัดการการรับ request จากผู้ใช้ และส่ง response กลับไปยังผู้ใช้
-   **ตัวอย่าง View**:
    -   **`index` view**: แสดงหน้าแรกของแอปพลิเคชัน โดยใช้ `HttpResponse` ส่งข้อความกลับไป
        
        `def index(request):
            return HttpResponse("This is the index page of polls app")` 
        
    -   **`detail` view**: แสดงรายละเอียดของ question โดยใช้ path parameter (`question_id`) ที่ส่งเข้ามา
        

        `def detail(request, question_id):
            return HttpResponse("You're looking at question %s." % question_id)` 
        
-   **การกำหนด URL**: ใช้ไฟล์ `/polls/urls.py` เพื่อกำหนด path ของแต่ละ view
    
    `urlpatterns = [
        path("", views.index, name="index"),
        path("<int:question_id>/", views.detail, name="detail"),
        path("<int:question_id>/results/", views.results, name="results"),
        path("<int:question_id>/vote/", views.vote, name="vote"),
    ]` 
    

### 5. **การสร้าง Template ใน Django**

-   **ความสำคัญ**: Template เป็นไฟล์ HTML ที่ใช้แสดงข้อมูลจาก views ให้กับผู้ใช้
-   **การสร้าง Template**:
    -   สร้างโฟลเดอร์ `/polls/templates/` และสร้างไฟล์ `index.html` ในโฟลเดอร์นี้
    -   ตัวอย่างโค้ดในไฟล์ template:
        
        `<html>
            <head>
            </head>
            <body>
                <h1>Lastest questions</h1>
                {% if latest_question_list %}
                    <ul>
                    {% for question in latest_question_list %}
                        <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
                    {% endfor %}
                    </ul>
                {% else %}
                    <p>No polls are available.</p>
                {% endif %}
            </body>
        </html>` 
        
-   **การตั้งค่า Template Path ใน `settings.py`**:
    -   เพิ่ม path ของโฟลเดอร์ templates ใน `settings.py`
        
        `import os
        SETTINGS_PATH = os.path.dirname(os.path.dirname(__file__))
        
        TEMPLATE_DIRS = (
            os.path.join(SETTINGS_PATH, 'templates'),
        )` 
        

### 6. **การใช้งาน HttpResponse และ Http404**

-   **ความสำคัญ**: การจัดการ response คือการตอบกลับ request ที่ผู้ใช้ส่งเข้ามา
-   **ตัวอย่าง HttpResponse**:
    -   ส่งข้อความกลับไปหา client เมื่อเข้า view นั้นๆ
        
        `def vote(request, question_id):
            return HttpResponse("You're voting on question %s." % question_id)` 
        
-   **การจัดการข้อผิดพลาดด้วย Http404**:
    -   เมื่อไม่พบข้อมูลที่ผู้ใช้ร้องขอ เช่น question ที่มี `question_id` ไม่อยู่ในฐานข้อมูล
        
        `from django.http import Http404
        def detail(request, question_id):
            try:
                question = Question.objects.get(pk=question_id)
            except Question.DoesNotExist:
                raise Http404("Question does not exist")
            return render(request, "detail.html", {"question": question})` 
        

### 7. **การทดสอบการใช้งาน Django Server**

-   **ความสำคัญ**: ทดสอบว่าเว็บสามารถทำงานได้ตามที่ตั้งค่าไว้
-   **ขั้นตอน**:
    -   ใช้คำสั่ง `python manage.py runserver` เพื่อเริ่มต้นเซิร์ฟเวอร์
    -   เปิด browser และพิมพ์ `http://127.0.0.1:8000/polls/` เพื่อตรวจสอบว่าเว็บแสดงผลได้ตามที่คาดไว้
-   **ผลลัพธ์**: จะเห็นหน้าเว็บตามที่ได้สร้างไว้ใน template

### 8. **การแก้ไขปัญหาการใช้ Hardcoded URL**

-   **ความสำคัญ**: การ hardcode URLs อาจทำให้เกิดปัญหาเมื่อเปลี่ยนแปลง URL หลักของโปรเจค ดังนั้นควรใช้ template tags เช่น `{% url %}` เพื่อทำให้ URL ถูกอัพเดทโดยอัตโนมัติ
-   **ตัวอย่าง**:
    -   แก้ไขจาก:
        
        `<li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>` 
        
    -   เป็น:
        
        
        `<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>`
