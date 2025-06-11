import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import org.json.JSONObject;

public class WeatherApp {

// Method to fetch weather data from OpenWeatherMap API  
public static String fetchWeather(String city, String apiKey) {  
    try {  
        String endpoint = "https://api.openweathermap.org/data/2.5/weather?q=" + city + "&appid=" + apiKey + "&units=metric";  
        URL url = new URL(endpoint);  
        HttpURLConnection con = (HttpURLConnection) url.openConnection();  
        con.setRequestMethod("GET");  

        BufferedReader in = new BufferedReader(new InputStreamReader(con.getInputStream()));  
        String inputLine;  
        StringBuffer content = new StringBuffer();  

        while ((inputLine = in.readLine()) != null) {  
            content.append(inputLine);  
        }  
        in.close();  
        con.disconnect();  

        return content.toString();  
    } catch (Exception e) {  
        return "ERROR FETCHING DATA: " + e.getMessage();  
    }  
}  

// Method to parse and display weather data  
public static void displayWeather(String jsonResponse) {  
    try {  
        JSONObject obj = new JSONObject(jsonResponse);  
        String city = obj.getString("name");  
        JSONObject main = obj.getJSONObject("main");  
        double temp = main.getDouble("temp");  
        double feelsLike = main.getDouble("feels_like");  
        int humidity = main.getInt("humidity");  

        System.out.println("----- WEATHER REPORT -----");  
        System.out.println("CITY: " + city);  
        System.out.println("TEMPERATURE: " + temp + " °C");  
        System.out.println("FEELS LIKE: " + feelsLike + " °C");  
        System.out.println("HUMIDITY: " + humidity + " %");  

        JSONObject weatherObj = obj.getJSONArray("weather").getJSONObject(0);  
        String description = weatherObj.getString("description").toUpperCase();  
        System.out.println("CONDITION: " + description);  
        System.out.println("--------------------------");  

    } catch (Exception e) {  
        System.out.println("ERROR PARSING WEATHER DATA: " + e.getMessage());  
    }  
}  

public static void main(String[] args) {  
    String city = "Delhi"; // You can change this  
    String apiKey = "your_api_key_here"; // Replace with your actual API key  

    String response = fetchWeather(city, apiKey);  
    displayWeather(response);  

    System.out.println("THANK YOU FOR USING THE WEATHER APP.");  
    System.out.println("HAVE A NICE DAY.");  
}

}
