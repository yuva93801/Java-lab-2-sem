package lab6;

public class Shop {
    private int material;
    private boolean available = false;

    // Method to get the material
    public synchronized int get() {
        while (!available) {
            try {
                wait(); // Wait until material is available
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt(); // Handle interrupt
            }
        }
        available = false;
        notifyAll(); // Notify all waiting threads
        return material;
    }

    // Method to put the material
    public synchronized void put(int value) {
        while (available) {
            try {
                wait(); // Wait if material is already available
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt(); // Handle interrupt
            }
        }
        material = value;
        available = true;
        notifyAll(); // Notify all waiting threads
    }

    public static class Producer extends Thread {
        private Shop shop;
        private int number;

        public Producer(Shop shop, int number) {
            this.shop = shop;
            this.number = number;
        }

        @Override
        public void run() {
            for (int i = 0; i < 10; i++) { // Produce 10 items
                shop.put(i); // Put item into the shop
                System.out.println("Producer " + this.number + " put: " + i);
                try {
                    sleep((int) (Math.random() * 100)); // Random sleep
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt(); // Handle interrupt
                }
            }
        }
    }

    public static class Consumer extends Thread {
        private Shop shop;
        private int number;

        public Consumer(Shop shop, int number) {
            this.shop = shop;
            this.number = number;
        }

        @Override
        public void run() {
            for (int i = 0; i < 10; i++) { // Consume 10 items
                int value = shop.get(); // Get item from the shop
                System.out.println("Consumer " + this.number + " got: " + value);
                try {
                    sleep((int) (Math.random() * 100)); // Random sleep
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt(); // Handle interrupt
                }
            }
        }
    }

    public static void main(String[] args) {
        Shop shop = new Shop(); // Instantiate Shop object
        Producer producer = new Producer(shop, 1); // Create a Producer
        Consumer consumer = new Consumer(shop, 1); // Create a Consumer

        producer.start(); // Start the Producer thread
        consumer.start(); // Start the Consumer thread
    }
}
