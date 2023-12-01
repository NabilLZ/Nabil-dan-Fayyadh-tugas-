# Nabil-dan-Fayyadh-tugas-
Kelas Sistem Informasi - E
Muhammad Nabil Fatahilah - 235150401111063
Fayyadh Radhwa Ferdia - 235150401111058
Tugas Pemrograman Dasar - Pak Erik

import processing.core.PApplet;

public class CityTheme1 extends PApplet {

    Car[] cars;
    Cloud[] clouds;
    Dust[] dustParticles;

    float carX; 
    float carSpeed = 1; 
    
    float sunX; 
    float sunSpeed = 50; 
    int sunDelay = 40; 
    int sunCounter = 59; 
    int xs = 10, ys = 10;

    public static void main(String[] args) {
        PApplet.runSketch(new String[] { "City Theme" }, new CityTheme1());
    }
    public void sunmoon() {
        if (xs < 500) {
            fill(255, 255, 0);
            ellipse(xs += 25, ys -= 10, 75, 75);
        } else if (xs < 650) {
            fill(255, 255, 0);
            ellipse(xs += 25, ys -= 5, 75, 75);
        } else if (xs < 700) {
            fill(255, 255, 0);
            ellipse(xs += 25, ys, 75, 75);
        } else if (xs < 1000) {
            fill(255, 255, 0);
            ellipse(xs += 25, ys += 5, 75, 75);
        } else if (xs < 1300) {
            fill(255, 255, 0);
            ellipse(xs += 25, ys += 10, 75, 75);
        } else {
            fill(145, 142, 141);
            ellipse(xs += 25, ys += 10, 75, 75);
        }
    
        if (xs >= 1450) {
            xs = -120;
            ys = 275;
    
            float sunSize = map(xs, 500, 1000, 50, 150);
            ellipse(xs += 25, ys += 10, sunSize, sunSize);
        }
    }
    
    @Override
    public void settings() {
        size(800, 600);
        smooth();
    }

    @Override
    public void setup() {
        cars = new Car[5];
        clouds = new Cloud[3];
        dustParticles = new Dust[100]; 

        for (int i = 0; i < cars.length; i++) {
            float startX = random(width);
            float startY = random(300, 450);
            float speed = random(1, 2);

            cars[i] = new Car(startX, startY, speed);
        }

        for (int i = 0; i < clouds.length; i++) {
            float cloudX = random(width);
            float cloudY = random(50, 200);
            float cloudSpeed = random(1, 2);

            clouds[i] = new Cloud(cloudX, cloudY, cloudSpeed);
        }

        for (int i = 0; i < dustParticles.length; i++) {
            float dustX = random(width);
            float dustY = random(height);
            float dustSpeed = random(1, 1);

            dustParticles[i] = new Dust(dustX, dustY, dustSpeed);
        }

        sunX = width / 8;
    }

    @Override
    public void draw() {
        int x1 = 0;
        int y1 = 0;
        int x2 = 1300;
        int y2 = 0;
        int R1 = 255; 
        int G1 = 200; 
        int B1 = 0;   

        noStroke(); 

        for (int x = 0; x <= 380; x++) {
            fill(R1, G1, B1); 
            rect(x1, y1, x2, y2); 
            y1++;
            y2++;
            R1++; 
            G1++; 
            B1++; 
        }

        sunmoon(); 

        fill(165, 132, 97);
        noStroke();
        rect(0, 450, 800, 500);

        fill(64, 64, 64);
        stroke(0);
        rect(0, 300, 800, 150);

        drawBuildings();
        drawRoad();
        drawDust();

        for (int i = 0; i < cars.length; i++) {
            cars[i].update();
            cars[i].display();
        }

        for (int i = 0; i < clouds.length; i++) {
            clouds[i].update();
            clouds[i].display();
        }
    }

    void drawRoad() {
        drawTrees();

        drawCar(carX, height * 5 / 6);
        carX += carSpeed;

        if (carX > width + 50) {
            carX = -50;
        }

        drawDashedLine();

        for (int i = 0; i < cars.length; i++) {
            cars[i].update();
            cars[i].display();
        }
    }

    void drawDashedLine() {
        fill(255);
        float dashLength = 20;
        float gapLength = 10;
        float totalLength = width;

        for (float i = 0; i < totalLength; i += dashLength + gapLength) {
            rect(i, 375, dashLength, 4);
        }
    }

    void drawDust() {
        for (int i = 0; i < dustParticles.length; i++) {
            dustParticles[i].update();
            dustParticles[i].display();
        }
    }

    

    void drawSky() {
        fill(0, 0, 204);
        noStroke();
        rect(0, 0, width, height / 2);

        // Menggambar awan
        for (int i = 0; i < clouds.length; i++) {
            clouds[i].update();
            clouds[i].display();
        }
    }

    void drawBuildings() {
        fill(150, 100, 100);
        rect(50, height / 2 - 200, 100, 200);
        drawWindows(50, height / 2 - 200, 100, 200);

        fill(120, 150, 120);
        rect(250, height / 2 - 250, 150, 250);
        drawWindows(250, height / 2 - 250, 150, 250);

        fill(180, 180, 180);
        rect(500, height / 2 - 180, 120, 180);
        drawWindows(500, height / 2 - 180, 120, 180);

        fill(200, 150, 100);
        rect(700, height / 2 - 220, 80, 220);
        drawWindows(700, height / 2 - 220, 80, 220);
    }

    class Car {
        float x, y;
        float speed;

        Car(float x, float y, float speed) {
            this.x = x;
            this.y = y;
            this.speed = speed;
        }

        void update() {
            x += speed;

            if (x > width + 50) {
                x = -50;
                y = random(300, 440);
            }
        }

        void display() {
            stroke(0);
            fill(255, 0, 0);
            rect(x, y - 20, 60, 20);
            fill(255, 255, 255);
            rect(x + 10, y - 30, 40, 10);
            fill(0);
            ellipse(x + 15, y, 15, 15);
            ellipse(x + 45, y, 15, 15);
        }
    }

    class Cloud {
        float x, y;
        float speed;

        Cloud(float x, float y, float speed) {
            this.x = x;
            this.y = y;
            this.speed = speed;
        }

        void update() {
            x += speed;

            if (x > width + 100) {
                x = -100;
            }
        }

        void display() {
            fill(255);
            noStroke();
            ellipse(x, y, 50, 30);
            ellipse(x + 20, y - 10, 50, 30);
            ellipse(x + 40, y, 50, 30);
        }
    }

    // Fungsi untuk menggambar mobil
    class Dust {
        float x, y;
        float speed;

        Dust(float x, float y, float speed) {
            this.x = x;
            this.y = y;
            this.speed = speed;
        }

        void update() {
            y += speed;

            if (y > height) {
                y = 0;
            }
        }

        void display() {
            fill(200);
            noStroke();
            ellipse(x, y, 2, 2);
        }
    }

    void drawCar(float x, float y) {
        
    }
    void drawTree(float x, float y) {
        fill(139, 69, 19);
        stroke(0);
        rect(x - 5, y, 10, 60);

        fill(34, 139, 34);
        noStroke();
        ellipse(x, y - 20, 50, 50);
        ellipse(x - 15, y, 50, 50);
        ellipse(x + 15, y, 50, 50);
    }

    void drawTrees() {
        noStroke();
        drawTree(100, 450);
        drawTree(200, 500);
        drawTree(300, 450);
        drawTree(400, 500);
        drawTree(500, 450);
        drawTree(600, 500);
        drawTree(700, 450);
        drawTree(750, 500);
    }

    void drawWindows(float x, float y, float buildingWidth, float buildingHeight) {
        float windowWidth = 20; 
        float windowHeight = 30; 
        float gap = 10; 

        fill(255); 
        for (float i = x + gap; i < x + buildingWidth - windowWidth; i += windowWidth + gap) {
            for (float j = y + gap; j < y + buildingHeight - windowHeight; j += windowHeight + gap) {
                rect(i, j, windowWidth, windowHeight); 
            }
        }
    }
}
