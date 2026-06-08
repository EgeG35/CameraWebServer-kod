#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

Adafruit_SSD1306 display(
  SCREEN_WIDTH,
  SCREEN_HEIGHT,
  &Wire,
  -1
);

String durum = "KULLANICI_YOK";

unsigned long sonBip = 0;
unsigned long oturmaBaslangic = 0;

bool molaModu = false;
int renk = 0;

void yazEkran(String satir1, String satir2)
{
  display.clearDisplay();

  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);

  display.setCursor(0, 10);
  display.println(satir1);

  display.setCursor(0, 25);
  display.println(satir2);

  display.display();
}

void rgb(int r, int g, int b)
{
  digitalWrite(9, r);
  digitalWrite(10, g);
  digitalWrite(11, b);
}

void setup()
{
  Serial.begin(9600);

  pinMode(8, OUTPUT);

  pinMode(9, OUTPUT);
  pinMode(10, OUTPUT);
  pinMode(11, OUTPUT);

  if (!display.begin(
        SSD1306_SWITCHCAPVCC,
        0x3C))
  {
    while (true);
  }

  yazEkran(
    "ANKABOT",
    "SISTEM HAZIR"
  );

  rgb(0, 0, 0);
}

void loop()
{
  if (Serial.available())
  {
    String mesaj =
      Serial.readStringUntil('\n');

    mesaj.trim();

    durum = mesaj;

    if (durum == "KULLANICI_YOK")
    {
      rgb(0, 1, 0);

      noTone(8);

      molaModu = false;
      oturmaBaslangic = 0;
      renk = 0;

      yazEkran(
        "KULLANICI",
        "YOK"
      );
    }

    else if (durum == "NORMAL")
    {
      if (!molaModu)
      {
        rgb(0, 0, 1);

        yazEkran(
          "NORMAL",
          "OTURUS"
        );
      }

      if (oturmaBaslangic == 0)
      {
        oturmaBaslangic = millis();
      }
    }

    else if (durum == "DUZ_OTUR")
    {
      if (!molaModu)
      {
        rgb(1, 0, 0);

        yazEkran(
          "POSTURUNU",
          "DUZELT"
        );
      }

      if (oturmaBaslangic == 0)
      {
        oturmaBaslangic = millis();
      }
    }
  }

  // DURUS BOZUKSA HER 1 SANIYEDE BIR UYARI
  if (
    durum == "DUZ_OTUR" &&
    !molaModu
  )
  {
    if (
      millis() - sonBip >
      1000
    )
    {
      tone(8, 1000);
      delay(200);
      noTone(8);

      sonBip = millis();
    }
  }

  // 20 SANIYE SONRA MOLA
  if (
    !molaModu &&
    oturmaBaslangic > 0 &&
    millis() - oturmaBaslangic > 20000
  )
  {
    molaModu = true;

    yazEkran(
      "MOLA ZAMANI",
      "20-20-20"
    );

    sonBip = millis();
  }

  // MOLA MODU
  if (molaModu)
  {
    yazEkran(
      "MOLA ZAMANI",
      "20-20-20"
    );

    if (
      millis() - sonBip >
      2000
    )
    {
      tone(8, 1000);
      delay(300);
      noTone(8);

      renk++;

      if (renk == 1)
      {
        rgb(1, 0, 0); // Kirmizi
      }
      else if (renk == 2)
      {
        rgb(0, 1, 0); // Yesil
      }
      else if (renk == 3)
      {
        rgb(0, 0, 1); // Mavi
      }
      else if (renk == 4)
      {
        rgb(1, 1, 0); // Sari
      }
      else if (renk == 5)
      {
        rgb(1, 0, 1); // Mor
      }
      else
      {
        renk = 0;
      }

      sonBip = millis();
    }
  }
}
