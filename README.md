# planeseating

package seating;

import java.util.*;

public class PlaneSeatingFourPerson {

    public static int solution(int n, String s) {
        int totalNumberOfFamilyOfFours = 0;
        if (s.isEmpty()) {
            // if entire row is unreserved, max 2 family can be seated  in this arrangement "BCDE" and "FGHJ"
            totalNumberOfFamilyOfFours = 2 * n;
            System.out.println("Reserved seats:" + s + ":rows:" + n + "; Total number of family of four that can fit together in unreserved seats:" + totalNumberOfFamilyOfFours);
            return totalNumberOfFamilyOfFours;
        }
        Map<Integer, List<Character>> map = new HashMap<>();
        for (String occupiedSeat : s.split(" ")) {
            int row = Integer.parseInt(occupiedSeat.substring(0, occupiedSeat.length() - 1));
            char columnChar = occupiedSeat.substring(occupiedSeat.length() - 1).charAt(0);
            map.putIfAbsent(row, new ArrayList<>());
            map.get(row).add(columnChar); // row as key and list of reserved columns chars as value
        }
        // if entire row is unreserved, max 2 family can be seated  in this arrangement "BCDE" and "FGHJ"
        totalNumberOfFamilyOfFours = 2 * (n - map.size());
        for (Map.Entry<Integer, List<Character>> entry : map.entrySet()) {
            List<Character> seats = entry.getValue();
            boolean isLeftAisle = false, isRightAisle = false, isMiddle = false;
            for (char seat : seats) {
                // we can ignore A and K as it's not impacting seating arrangement
                if (seat >= 'B' && seat <= 'E')
                    isLeftAisle = true;
                if (seat >= 'F' && seat <= 'J')
                    isRightAisle = true;
                if (seat >= 'D' && seat <= 'G')
                    isMiddle = true;
                if (isLeftAisle && isRightAisle && isMiddle) {
                    // not possible for seating arrangement
                    break;
                }
            }
            if (!isLeftAisle)
                totalNumberOfFamilyOfFours += 1; // family can be fit together in BCDE
            if (!isRightAisle)
                totalNumberOfFamilyOfFours += 1; // family can be fit together in FGHJ
            if (isLeftAisle && isRightAisle && !isMiddle)
                totalNumberOfFamilyOfFours += 1; // family can be fit together in DEFG
        }
        System.out.println("Reserved seats:" + s + ":rows:" + n + "; Total number of family of four that can fit together in unreserved seats:" + totalNumberOfFamilyOfFours);
        return totalNumberOfFamilyOfFours;
    }

    public static void main(String[] args) {
        solution(2, "1A 2F 1C");
        solution(1, "");
        solution(22, "1A 3C 2B 20G 5A");
    }
}
