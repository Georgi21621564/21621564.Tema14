import java.util.HashMap;
import java.util.Map;

public class Theater {
    
    private Map<String, Map<String, Performance>> schedule = new HashMap<>();
    
    public void addPerformance(String date, String name, String hall) {
        if (schedule.containsKey(date) && schedule.get(date).containsKey(hall)) {
            System.out.println("Error: Another performance is already scheduled for this hall on the same date.");
            return;
        }
        
        Performance performance = new Performance(name, hall);
        if (!schedule.containsKey(date)) {
            schedule.put(date, new HashMap<>());
        }
        schedule.get(date).put(hall, performance);
    }
    
    public String[] getAvailableSeats(String date, String name) {
        if (!schedule.containsKey(date) || !schedule.get(date).containsKey(name)) {
            return new String[0];
        }
        
        Performance performance = schedule.get(date).get(name);
        return performance.getAvailableSeats();
    }
    
    private class Performance {
        private String name;
        private String hall;
        private Map<String, Boolean> seats;
        
        public Performance(String name, String hall) {
            this.name = name;
            this.hall = hall;
            this.seats = new HashMap<>();
            for (int i = 1; i <= 10; i++) {
                for (int j = 1; j <= 10; j++) {
                    seats.put(i + "-" + j, true);
                }
            }
        }
        
        public String[] getAvailableSeats() {
            int count = 0;
            for (boolean available : seats.values()) {
                if (available) {
                    count++;
                }
            }
            
            String[] availableSeats = new String[count];
            int index = 0;
            for (Map.Entry<String, Boolean> entry : seats.entrySet()) {
                if (entry.getValue()) {
                    availableSeats[index++] = entry.getKey();
                }
            }
            
            return availableSeats;
        }
    }
    
    public static void main(String[] args) {
        Theater theater = new Theater();
        theater.addPerformance("2023-03-20", "Hamlet", "Hall1");
        theater.addPerformance("2023-03-21", "Macbeth", "Hall2");
        String[] availableSeats = theater.getAvailableSeats("2023-03-20", "Hamlet");
        System.out.println("Available seats for Hamlet on 2023-03-20:");
        for (String seat : availableSeats) {
            System.out.println(seat);
        }
    }
}
