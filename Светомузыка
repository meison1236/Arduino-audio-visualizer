Код программы: 

#include <LedControl.h>  // Библиотека для управления светодиодной матрицей MAX7219
#include <FixFFT.h>      // Библиотека для выполнения быстрого преобразования Фурье (FFT)

// Пины для подключения MAX7219 (DIN, CLK, CS)
int DIN = 12;  
int CLK = 11;  
int CS = 10;   
LedControl lc = LedControl(DIN, CLK, CS, 1); // Инициализация одного модуля MAX7219

// Пин для подключения AUX
#define AUX_PIN A0       // Аналоговый вход для аудиосигнала с AUX 

// Переменные для обработки звука
#define SAMPLES 64       // Количество сэмплов для FFT 
char im[SAMPLES];        // Массив для мнимой части FFT
char data[SAMPLES];      // Массив для реальной части FFT
int bass, mid, high;     // Переменные для хранения уровней частот

// Переменные для режимов работы
int mode = 0;            // Текущий режим: 0 - Волны, 1 - Частоты, 2 - Цветомузыка
const int brightness = 8; // Фиксированная яркость матрицы (0-15)

// Настройка устройства
void setup() {
  // Инициализация матрицы MAX7219
  lc.shutdown(0, false);     // Выключаем режим энергосбережения для модуля 0
  lc.setIntensity(0, brightness); // Устанавливаем фиксированную яркость
  lc.clearDisplay(0);        // Очищаем дисплей

  // Настройка пина AUX
  pinMode(AUX_PIN, INPUT);   // Устанавливаем пин AUX как вход
}

// Основной цикл программы
void loop() {
 checkModeChange(); // Проверка смены режима
  readAudio();           // Считывание и обработка аудиосигнала с AUX
  updateDisplay();       // Обновление светодиодной матрицы в зависимости от режима
  delay(50);             // Задержка для управления скоростью обновления
}
// Функция для проверки смены режима
void checkModeChange() {
  static unsigned long lastPress = 0;
  if (digitalRead(MODE_BUTTON) return

  // Защита от дребезга
  if (millis() - lastPress < 300) return;
  lastPress = millis();  
  changeMode((mode + 1) % 3); // Циклическое переключение 0→1→2→0
  Serial.print("Режим изменен на: ");
  Serial.println(mode);
}

void changeMode(int newMode) {
  mode = newMode;
  lc.clearDisplay(0); // Очистка при смене режима
}

// Функция для считывания и обработки аудиосигнала с AUX
void readAudio() {
  // Сбор данных с AUX
  for (int i = 0; i < SAMPLES; i++) {
    int rawSignal = analogRead(AUX_PIN); // Считываем сигнал с AUX (0-1023)
    data[i] = (rawSignal - 512) / 4;     // Нормализуем сигнал: вычитаем смещение (512 - среднее значение) и приводим к диапазону -128..127
    im[i] = 0;                           // Мнимая часть для FFT изначально равна 0
    delayMicroseconds(50);               // Небольшая задержка между сэмплами для стабильности
  }

  // Выполнение FFT
  fix_fft(data, im, 6, 0); // Преобразование Фурье (6 = log2(SAMPLES))

  // Вычисление амплитуд для частотных диапазонов
  bass = 0; mid = 0; high = 0;
  for (int i = 0; i < SAMPLES / 2; i++) {
    int amplitude = sqrt(data[i] * data[i] + im[i] * im[i]); // Вычисляем амплитуду
    if (i < 4) bass += amplitude;         // Низкие частоты (0-3)
    else if (i < 16) mid += amplitude;    // Средние частоты (4-15)
    else high += amplitude;               // Высокие частоты (16+)
  }
  // Нормализация значений для использования с матрицей (0-7)
  bass = map(bass, 0, 500, 0, 7);
  mid = map(mid, 0, 1000, 0, 7);
  high = map(high, 0, 800, 0, 7);
}

// Функция для обновления светодиодной матрицы
void updateDisplay() {
  switch (mode) {
    case 0: // Режим "Волны" 
      for (int row = 0; row < 8; row++) {
        if (row <= bass) {
          for (int col = 0; col < 8; col++) {
            lc.setLed(0, row, col, true); // Зажигаем светодиоды в зависимости от уровня басов
          }
        }
      }
      break;

    case 1: // Режим "Частоты" - разделение на зоны
      for (int col = 0; col < 8; col++) {
        if (col < 3) { // Зона низких частот
          for (int row = 7; row > 7 - bass; row--) lc.setLed(0, row, col, true);
        }
        else if (col < 6) { // Зона средних частот
          for (int row = 7; row > 7 - mid; row--) lc.setLed(0, row, col, true);
        }
        else { // Зона высоких частот
          for (int row = 7; row > 7 - high; row--) lc.setLed(0, row, col, true);
        }
      }
      break;

    case 2: // Режим "Цветомузыка" 
      for (int row = 0; row < 8; row++) {
        if (row < bass) lc.setLed(0, row, 0, true); // Басы - первая колонка
        if (row < mid) lc.setLed(0, row, 4, true);  // Средние - средняя колонка
        if (row < high) lc.setLed(0, row, 7, true); // Высокие - последняя колонка
      }
      break;
  }
}

