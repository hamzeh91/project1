
import android.util.Log;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;
import java.nio.charset.Charset;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;

public class QueryUtil {

    /**
     * Tag for the log messages
     */
    private static final String LOG_TAG = QueryUtil.class.getSimpleName();

    public static ArrayList<Books> searchResults(String searchUrl) {
        ArrayList<Books> booksArrayList = new ArrayList<>();

        URL queryURL = null;
        String jsonString = null;

        // Create URL object

        try {
            queryURL = new URL(searchUrl);
            Log.v(LOG_TAG , "queryURL = "+queryURL);

        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        // Perform HTTP request to the URL and receive a JSON response back

        try {
             jsonString = httpRequest(queryURL);
            Log.v(LOG_TAG , "jsonString = "+jsonString);

        } catch (IOException e) {
            Log.e(LOG_TAG, "Problem making the HTTP request.", e);
        }

        // Try to parse the SAMPLE_JSON_RESPONSE. If there's a problem with the way the JSON
        // is formatted, a JSONException exception object will be thrown.
        // Catch the exception so the app doesn't crash, and print the error message to the logs.

        try {
            assert jsonString != null;
            JSONObject rootJason = new JSONObject(jsonString);
            JSONArray itemsArray = rootJason.getJSONArray("items");

           for ( int i =0 ; i<itemsArray.length();i++){

               JSONObject arrayItem = itemsArray.getJSONObject(i);
               JSONObject volumeInfo = arrayItem.getJSONObject("volumeInfo");
               String title = volumeInfo.getString("title");
               String subtitle = volumeInfo.getString("subtitle");
               JSONArray authorArray = volumeInfo.getJSONArray("authors");
               String  author = authorArray.getString(i);
               String infoLink = volumeInfo.getString("infoLink");
               JSONObject imageLinks = volumeInfo.getJSONObject("imageLinks");
               String thumbnail = imageLinks.getString("thumbnail");

               booksArrayList.add(new Books(title,subtitle,author,infoLink,thumbnail));


           }



        } catch (JSONException e) {
            e.printStackTrace();
        }

        return booksArrayList;
    }


    private static String httpRequest(URL url) throws IOException {

        String jsonResponse = null;
        HttpURLConnection urlConnection = null;
        InputStream inputStream = null;
        // If the URL is null, then return early.
        if (url == null) {
            return jsonResponse;
        }

        try {
            urlConnection = (HttpURLConnection) url.openConnection();
            urlConnection.setRequestMethod("GET");
            urlConnection.setReadTimeout(10000 /* milliseconds */);
            urlConnection.setConnectTimeout(15000 /* milliseconds */);
            urlConnection.connect();

            int resposeCode = urlConnection.getResponseCode();
            Log.v("ResponseCode", " =" + resposeCode);

            // If the request was successful (response code 200),
            // then read the input stream and parse the response.
            if (resposeCode == 200) {
                inputStream = urlConnection.getInputStream();
                jsonResponse = readFromStream(inputStream);

            } else {
                Log.e(LOG_TAG, "Error response code: " + urlConnection.getResponseCode());

            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (urlConnection != null) {
                urlConnection.disconnect();
            }
            if (inputStream != null) {
                // Closing the input stream could throw an IOException, which is why
                // the makeHttpRequest(URL url) method signature specifies than an IOException
                // could be thrown.
                inputStream.close();
            }
        }
        return jsonResponse;

    }


    /**
     * Convert the {@link InputStream} into a String which contains the
     * whole JSON response from the server.
     */
    private static String readFromStream(InputStream inputStream) throws IOException {

        StringBuilder output = new StringBuilder();

        if (inputStream != null) {

            InputStreamReader inputStreamReader = new InputStreamReader(inputStream, Charset.forName("UTF-8"));
            BufferedReader reader = new BufferedReader(inputStreamReader);

            String line = reader.readLine();
            while (line != null) {
                output.append(line);
                line = reader.readLine();
                Log.v(LOG_TAG,"buffering = "+output);

            }

        }
        return output.toString();

    }

}
