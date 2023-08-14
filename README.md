# MY-Assignment
This is the assignment that i had did based and using maven and spring boot
How the generally Integration works
Certainly! Here's a step-by-step explanation of how the integration works.
Step 1: Set Up Webhooks in Asana
Create a webhook in Asana that listens for task creation events. When a new task is created, Asana will send a POST request to your specified endpoint.
Step 2: Create an Airtable API Key
Obtain an API key from Airtable by signing up on their website and creating a new base with a table named "Asana Tasks".
Step 3: Writing Java Code for the Integration
import org.springframework.http.*;
import org.springframework.web.client.RestTemplate;
import java.util.HashMap;
import java.util.Map;
public class AsanaAirtableIntegration {
    public static void main(String[] args) {
        String taskID = "12345";
        String taskName = "Sample Task";
        String assignee = "John Doe";
        String dueDate = "2023-08-31";
        String description = "Sample description";
        copyTaskToAirtable(taskID, taskName, assignee, dueDate, description);
    }
    public static void copyTaskToAirtable(String taskID, String taskName, String assignee, String dueDate, String description) {
        String airtableEndpoint = "https://api.airtable.com/v0/YOUR_BASE_ID/Asana%20Tasks";
        String airtableApiKey = "YOUR_AIRTABLE_API_KEY";
        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", "Bearer " + airtableApiKey);
        headers.setContentType(MediaType.APPLICATION_JSON);
        Map<String, Object> fields = new HashMap<>();
        fields.put("Task ID", taskID);
        fields.put("Name", taskName);
        fields.put("Assignee", assignee);
        fields.put("Due Date", dueDate);
        fields.put("Description", description);
        Map<String, Object> requestBody = new HashMap<>();
        requestBody.put("fields", fields);
        HttpEntity<Map<String, Object>> requestEntity = new HttpEntity<>(requestBody, headers);
        RestTemplate restTemplate = new RestTemplate();
        ResponseEntity<String> responseEntity = restTemplate.exchange(
                airtableEndpoint,
                HttpMethod.POST,
                requestEntity,
                String.class
        );
        if (responseEntity.getStatusCode() == HttpStatus.CREATED) {
            System.out.println("Task copied to Airtable successfully");
        } else {
            System.out.println("Error copying task to Airtable");
        }
    }
}
Make sure to replace YOUR_BASE_ID with your actual Airtable base ID and YOUR_AIRTABLE_API_KEY with your Airtable API key.
