# reconocimiento-de-texto-con-webcam-y-python


import cv2
import easyocr
import os

# Iniciar la cámara
cam = cv2.VideoCapture(1)  # Usa 0 si tienes solo una cámara
print("Presiona 'ESPACIO' para capturar la imagen...")

while True:
    ret, frame = cam.read()
    if not ret:
        print("Error: No se pudo acceder a la cámara.")
        break

    cv2.imshow("Captura", frame)

    # Presionar ESPACIO para capturar
    if cv2.waitKey(1) & 0xFF == ord(' '):
        cv2.imwrite("captura.jpg", frame)
        print("Imagen capturada y guardada como 'captura.jpg'.")
        break

# Liberar cámara y cerrar ventana
cam.release()
cv2.destroyAllWindows()

# Verificar si la imagen fue guardada
if not os.path.exists("captura.jpg"):
    print("Error: No se pudo guardar la imagen.")
    exit()

# Leer la imagen
img = cv2.imread("captura.jpg")

# Mostrar la imagen capturada
cv2.imshow("Imagen Capturada", img)
cv2.waitKey(2000)  # Mostrar la imagen por 2 segundos
cv2.destroyAllWindows()

# Inicializar EasyOCR en español
reader = easyocr.Reader(['es'])  # 'es' para idioma español

# Aplicar OCR a la imagen
texto = reader.readtext("captura.jpg", detail=0)  # detail=0 para obtener solo el texto

# Mostrar el resultado
print("Texto extraído:", texto if texto else "No se detectó texto.")
