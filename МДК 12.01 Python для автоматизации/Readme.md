https://replit.com/@OlgaChubova205/6semestr#main.py https://colab.research.google.com/drive/1EIG4FzRFcI642aPmcASVTNl4AoDmo5zU

https://colab.research.google.com/drive/19lA79XCI8aYRIK1TNqn5OrnoYcAa2nty?usp=sharing https://drive.google.com/drive/my-drive

#1 задать список A = ['1 thousand devils', 'My name is 9Pasha', 'Room #125 costs $100', '888'] result=[] Result_new = [] #2 список превратить в строку - разбить список на элементы for i in A: #разделить строку на слова Words = i.split() print("Words") print(Words)

for word in Words: #4 убрать спец. символы и цифры print (word.isalpha()) if word.isalpha(): result = word #5 объединение в одно result_new=" ".join(result) Result_new.append(result_new) print (result) print(Result_new)

#2 вариант запись генератор result = list(map(lambda n : ' '.join(filter(lambda n : n.isalpha(), n.split())), A)) print (result)

https://colab.research.google.com/drive/1ngjAq73u_Epq48y-rlVe6-P896qG7E8Z#scrollTo=dOBDcQMbOz0k import cv2

vid_capture = cv2.VideoCapture(0) if (vid_capture.isOpened() == False): print("Ошибка открытия видеофайла")

Чтение fps и количества кадров
else:

Получить информацию о частоте кадров
Можно заменить 5 на CAP_PROP_FPS, это перечисления
fps = vid_capture.get(5) print('Фреймов в секунду: ', fps,'FPS')

Получить количество кадров
Можно заменить 7 на CAP_PROP_FRAME_COUNT, это перечисления
frame_count = vid_capture.get(7) print('Частота кадров: ', frame_count) print('\n-----------------------------\nДля завершения нажмите "q" или Esc...')

file_count = 0 while (vid_capture.isOpened()): # Метод vid_capture.read() возвращают кортеж, первым элементом является логическое значение # а вторым кадр ret, frame = vid_capture.read()

if ret == True:




    # Gray, blur, adaptive threshold
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    print("gray")
    cv2.imshow("gray", gray)
    # https://medium.com/analytics-vidhya/gaussian-blurring-with-python-and-opencv-ba8429eb879b
    blur = cv2.GaussianBlur(gray, (33, 33), 0)
    print("blur")
    cv2.imshow("blur", blur)
    thresh = cv2.threshold(blur, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)[1]
    print(cv2.THRESH_BINARY, cv2.THRESH_OTSU)
    print("thresh")
    cv2.imshow("thresh", thresh)

    # Morphological transformations https://docs.opencv.org/4.x/d9/d61/tutorial_py_morphological_ops.html
    kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (5, 5))
    print("kernel")
    cv2.imshow("kernel", kernel)
    opening = cv2.morphologyEx(thresh, cv2.MORPH_OPEN, kernel)
    print("opening")
    cv2.imshow("opening", opening)
    # Find contours https://learnopencv.com/contour-detection-using-opencv-python-c/
    # CHAIN_APPROX_SIMPLE
    # Алгоритм сжимает горизонтальные, вертикальные и диагональные сегменты вдоль контура и оставляет только их конечные точки.
    # Это означает, что любая из точек вдоль прямых путей будет отклонена, и у нас останутся только конечные точки.
    cnts = cv2.findContours(thresh, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    cnts = cnts[0] if len(cnts) == 2 else cnts[1]

    for c in cnts:
        # Find perimeter of contour
        perimeter = cv2.arcLength(c, True)
        # Perform contour approximation
        approx = cv2.approxPolyDP(c, 0.04 * perimeter, True)
        # print(approx)
        # We assume that if the contour has more than a certain
        # number of verticies, we can make the assumption
        # that the contour shape is a circle
        if len(approx) > 0:

            # Obtain bounding rectangle to get measurements
            x, y, w, h = cv2.boundingRect(c)

            # Find measurements
            diameter = w
            radius = w / 2

            # Find centroid
            M = cv2.moments(c)

            if M["m00"] != 0:
                cX = int(M["m10"] / M["m00"])
                cY = int(M["m01"] / M["m00"])
            # print(cX,cY)
            # Draw the contour and center of the shape on the image
            # cv2.rectangle(image,(x,y),(x+w,y+h),(0,255,0),4)
            cv2.drawContours(frame, [c], 0, (36, 255, 12), 1)
            # cv2.circle(image, (cX, cY), 15, (320, 159, 22), -1)

            # Draw line and diameter information
            # cv2.line(image, (x, y + int(h/2)), (x + w, y + int(h/2)), (156, 188, 24), 3)
            # cv2.putText(image, "Diameter: {}".format(diameter), (cX - 150, cY - 50), cv2.FONT_HERSHEY_SIMPLEX, 1, (156, 188, 24), 3)

    cv2.imshow("frame" ,frame)
    cv2.imwrite('thresh.png', thresh)
    # cv2_imshow(thresh)
    cv2.imwrite('opening.png', opening)
    # cv2_imshow(opening)
    file_count += 1
    print('Кадр {0:04d}'.format(file_count))
    # writefile = 'Resources/Image_sequence/is42_{0:04d}.jpg'.format(file_count)
    # cv2.imwrite(writefile, frame)
    # 20 в миллисекундах, попробуйте увеличить значение, скажем, 50 и
    # понаблюдайте за изменениями в показе
    key = cv2.waitKey(20)

    thresh = cv2.threshold(blur, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)[1]
    print(cv2.THRESH_BINARY, cv2.THRESH_OTSU)
    print("thresh")

    cnts = cv2.findContours(thresh, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    cnts = cnts[0] if len(cnts) == 2 else cnts[1]
    i = 0


    frame1 = frame + 1
    before = frame
    after = frame1
    filled_after = after.copy()
    for c in cnts:
        # Find perimeter of contour
        perimeter = cv2.arcLength(c, True)
        # Perform contour approximation
        approx = cv2.approxPolyDP(c, 0.04 * perimeter, True)
        # print(approx)
        # We assume that if the contour has more than a certain
        # number of verticies, we can make the assumption
        # that the contour shape is a circle
        if len(approx) > 0:
            # Python program to explain cv2.putText() method

            # Reading an image in default mode

            if M["m00"] != 0:
                cX = int(M["m10"] / M["m00"])
                cY = int(M["m01"] / M["m00"])
                # print(cX,cY)
                # Draw the contour and center of the shape on the image
                # cv2.rectangle(image,(x,y),(x+w,y+h),(0,255,0),4)
            cv2.drawContours(frame, [c], 0, (36, 255, 12), 1)
            # cv2.circle(image, (cX, cY), 15, (320, 159, 22), -1)

            # Draw line and diameter information
            # cv2.line(image, (x, y + int(h/2)), (x + w, y + int(h/2)), (156, 188, 24), 3)
            # cv2.putText(image, "Diameter: {}".format(diameter), (cX - 150, cY - 50), cv2.FONT_HERSHEY_SIMPLEX, 1, (156, 188, 24), 3)
            # font
            font = cv2.FONT_HERSHEY_SIMPLEX

            # org
            org = (cX, cY)

            # fontScale
            fontScale = 1

            # Blue color in BGR
            color = (255, 0, 0)

            # Line thickness of 2 px
            thickness = 2

            # Using cv2.putText() method
            image = cv2.putText(frame, str(i), org, font, fontScale, color, thickness, cv2.LINE_AA)
            i = i + 1
            # Displaying the image

            # Obtain bounding rectangle to get measurements
            x, y, w, h = cv2.boundingRect(c)

            # Find measurements
            diameter = w
            radius = w / 2

            # Find centroid
            M = cv2.moments(c)

    cv2.imshow("frame1", frame)
    cv2.imwrite('image.png', frame)

    cv2.imwrite('thresh.png', thresh)
    # cv2_imshow(thresh)
    cv2.imwrite('opening.png', opening)
    # cv2_imshow(opening)
    cv2.imshow("fr1", filled_after)
    for c in cnts:
        # Find perimeter of contour
        perimeter = cv2.arcLength(c, True)
        # Perform contour approximation
        approx = cv2.approxPolyDP(c, 0.04 * perimeter, True)
        # print(approx)
        # We assume that if the contour has more than a certain
        # number of verticies, we can make the assumption
        # that the contour shape is a circle
        if len(approx) > 0:
            # Python program to explain cv2.putText() method

            # Reading an image in default mode

            if M["m00"] != 0:
                cX = int(M["m10"] / M["m00"])
                cY = int(M["m01"] / M["m00"])
                # print(cX,cY)
                # Draw the contour and center of the shape on the image
                # cv2.rectangle(image,(x,y),(x+w,y+h),(0,255,0),4)
            cv2.drawContours(filled_after, [c], 0, (36, 255, 12), 1)
            # cv2.circle(image, (cX, cY), 15, (320, 159, 22), -1)

            # Draw line and diameter information
            # cv2.line(image, (x, y + int(h/2)), (x + w, y + int(h/2)), (156, 188, 24), 3)
            # cv2.putText(image, "Diameter: {}".format(diameter), (cX - 150, cY - 50), cv2.FONT_HERSHEY_SIMPLEX, 1, (156, 188, 24), 3)
            # font
            font = cv2.FONT_HERSHEY_SIMPLEX

            # org
            org = (cX, cY)

            # fontScale
            fontScale = 1

            # Blue color in BGR
            color = (255, 0, 0)

            # Line thickness of 2 px
            thickness = 2

            # Using cv2.putText() method
            image = cv2.putText(filled_after, str(i), org, font, fontScale, color, thickness, cv2.LINE_AA)
            i = i + 1
            # Displaying the image

            # Obtain bounding rectangle to get measurements
            x, y, w, h = cv2.boundingRect(c)

            # Find measurements
            diameter = w
            radius = w / 2

            # Find centroid
            M = cv2.moments(c)
    if (key == ord('q')) or key == 27:
        break
else:
    break
import cv2 as cv # Добавляет модуль для подключения видео cap = cv.VideoCapture(0) # Виртуальная камера if not cap.isOpened(): # Если нет видео print("Cannot open camera") # Выводим: видео отсутствует exit() while True: # Capture frame-by-frame # Виртуальная камера определяет кадры на видео ret, frame = cap.read() # if frame is read correctly ret is True Проверяет корректность кадра if not ret: # если кадр нек крректный print("Can't receive frame (stream end?). Exiting ...") # Выводим: невозможно вывести кадр break # Завершить программ

    для видео
import cv2 as cv # Добавляет модуль для подключения видео
cap = cv.VideoCapture(0) # Виртуальная камера
if not cap.isOpened(): # Если нет видео
   print("Cannot open camera") # Выводим: видео отсутствует
   exit()
while True:
   # Capture frame-by-frame # Виртуальная камера определяет кадры на видео
   ret, frame = cap.read()
   # if frame is read correctly ret is True Проверяет корректность кадра
   if not ret: # если кадр нек крректный
       print("Can't receive frame (stream end?). Exiting ...") # Выводим: невозможно вывести кадр
       break # Завершить программу



   image = frame # Работаем с кадром


   # Gray, blur, adaptive threshold
   gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY) # Превращает основные цвета в оттенки серого

   # cv2_imshow(gray)
   blur = cv.GaussianBlur(gray, (3, 3), 0) # Размытие всего кадра
   # cv2_imshow(blur)
   thresh = cv.threshold(blur, 0, 255, cv.THRESH_BINARY + cv.THRESH_OTSU)[1] # пытается определить границы
   # cv2_imshow(thresh)

   # Morphological transformations
   kernel = cv.getStructuringElement(cv.MORPH_RECT, (5, 5)) # Определяет элемент
   opening = cv.morphologyEx(thresh, cv.MORPH_OPEN, kernel) # задает границу
   # cv2_imshow(opening)
   # Find contours
   cnts = cv.findContours(opening, cv.RETR_TREE, cv.CHAIN_APPROX_SIMPLE) # Находит контуры
   cnts = cnts[0] if len(cnts) == 2 else cnts[1] # Ищет все контуры

   for c in cnts:
       # Find perimeter of contour
       perimeter = cv.arcLength(c, True)
       # Perform contour approximation
       approx = cv.approxPolyDP(c, 0.04 * perimeter, True) # обозначает конгтур и задает ему дальнейший путь
       # print(approx)
       # We assume that if the contour has more than a certain
       # number of verticies, we can make the assumption
       # that the contour shape is a circle
       if len(approx) > 3: # если контур содержит более т3 точек, принимаем его за круг
           # Obtain bounding rectangle to get measurements
           x, y, w, h = cv.boundingRect(c) # диаметр, радиус, ширина, высота объекта

           # Find measurements
           diameter = w
           radius = w / 2

           # Find centroid
           M = cv.moments(c)
           cX = int(M["m10"] / M["m00"]) # Координаты центра контура
           cY = int(M["m01"] / M["m00"])
           # print(cX,cY)
           # Draw the contour and center of the shape on the image
           cv.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 4) # Прямоугольный контур
           cv.drawContours(image, [c], 0, (36, 255, 12), 4) # рисование контура на изображении
           cv.circle(image, (cX, cY), 5, (320, 159, 22), -1) # Поставить точку в центре

           # Draw line and diameter information
           cv.line(image, (x, y + int(h / 2)), (x + w, y + int(h / 2)), (156, 188, 24), 3) # рИСОВАНИЕ ЛИНИИ ЧЕРЕЗ ЦЕТР ОБЪЕКТА
           cv.putText(image, "Diameter: {}".format(diameter), (cX - 150, cY - 50), cv.FONT_HERSHEY_SIMPLEX, 0.5,
                       (156, 188, 24), 1) # вЫВОД ТЕКСТА НА ИЗОБРАЖЕНИЕ


   cv.imshow("itog", image) #отображает изображение в окне

   # cv2_imshow(thresh)

   # cv2_imshow(opening)

   # Our operations on the frame come here

   if cv.waitKey(1) == ord('q'): # Ожидает нажатия клавиши для завершения работы программы
       break
# When everything done, release the capture
cap.release() # Освобождает ресурсы, используемые программой для видеопотока
cv.destroyAllWindows() # закрывавет все окна программы
# When everything done, release the capture
cap.release() # Освобождает ресурсы, используемые программой для видеопотока
cv.destroyAllWindows() # закрывавет все окна программы
https://colab.research.google.com/drive/19lA79XCI8aYRIK1TNqn5OrnoYcAa2nty?usp=sharing&authuser=1#scrollTo=HE228xwJsUb- image image image image

нейронки

1

image

2

image

3

image

4

image

5

https://colab.research.google.com/drive/1ud9sbK8dV5ooe9K2Ez2NUxLknPlf4GLx?usp=sharing

import cv2 as cv # Добавляет модуль для подключения видео
cap = cv.VideoCapture(0) # Виртуальная камера
if not cap.isOpened(): # Если нет видео
   print("Cannot open camera") # Выводим: видео отсутствует
   exit()
while True:
   # Capture frame-by-frame # Виртуальная камера определяет кадры на видео
   ret, frame = cap.read()
   # if frame is read correctly ret is True Проверяет корректность кадра
   if not ret: # если кадр нек крректный
       print("Can't receive frame (stream end?). Exiting ...") # Выводим: невозможно вывести кадр
       break # Завершить программу
   import cv2

   img = frame
   bd = cv2.barcode.BarcodeDetector()
   # bd = cv2.barcode.BarcodeDetector('path/to/sr.prototxt', 'path/to/sr.caffemodel')

   retval, decoded_info, decoded_type, points = bd.detectAndDecode(img)
   print(retval)
   print(decoded_info)
   print(decoded_type)
   print(cv2.barcode.EAN_13)
   img = cv2.polylines(img, points.astype(int), True, (0, 255, 0), 3)
   for s, p in zip(decoded_info, points):
       img = cv2.putText(img, s, p[1].astype(int),
                     cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 0, 255), 1, cv2.LINE_AA)
   cv2.imshow("штрих код", img)

   print(type(points))
   print(points)
   print(points.shape)
   if cv.waitKey(0) == ord('q'):  # Ожидает нажатия клавиши для завершения работы программы

    break
# When everything done, release the capture
cap.release() # Освобождает ресурсы, используемые программой для видеопотока
cv.destroyAllWindows() # закрывавет все окна программы
# When everything done, release the capture
cap.release() # Освобождает ресурсы, используемые программой для видеопотока
cv.destroyAllWindows() # закрывавет все окна программы
https://drive.google.com/file/d/1-ZMeNKVViBsazvFFx57F9uCn9oJ3UPFn/view?usp=sharing https://drive.google.com/file/d/1-eSFufJxzCRx7WwK1qjNTY26vSR02K0q/view?usp=sharing
![237942050-23a5e15c-91ec-462c-af0f-83f85ede3750 pngваавиваива](https://github.com/alyysaveleva/6Sem/assets/131712175/d8d8b07f-5e46-47b7-9dd3-0cdc3269554c)
![ваиавива](https://github.com/alyysaveleva/6Sem/assets/131712175/e148feb7-c4c6-4fdd-bf27-ef53736748ec)
![авиваива](https://github.com/alyysaveleva/6Sem/assets/131712175/018a6172-8256-400d-b850-002d62aba6ae)
![виваива](https://github.com/alyysaveleva/6Sem/assets/131712175/cae97d7d-c898-4890-9f3a-38c016f1c30e)
![оорооро](https://github.com/alyysaveleva/6Sem/assets/131712175/0541e2c2-3ab6-4e18-94fc-d239de9866ea)
![порипт](https://github.com/alyysaveleva/6Sem/assets/131712175/947e095f-8e5d-48d1-b90b-d84256a9b93d)
![алоалрро](https://github.com/alyysaveleva/6Sem/assets/131712175/d8632982-c19a-434a-968c-89d335d5efc8)
![пиралвалодыдож](https://github.com/alyysaveleva/6Sem/assets/131712175/a65fd21a-ff22-4994-98d6-0af9047c09a7)


















![241179903-cf481fac-5532-4649-ab64-c454f0de12af](https://github.com/alyysaveleva/6Sem/assets/131712175/71dcd1e1-e754-42e3-9019-219c88a7957c)

