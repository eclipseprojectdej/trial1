trial1
======
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.HttpURLConnection;
import java.net.URL;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@SuppressWarnings("serial")
public class ServletExample extends HttpServlet {

HttpServletRequest request;

public void doGet(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException {

    request = req;

    String strRequestIndex = request.getParameter("requestIndex");
    int intRequestIndex = 0;

    if (strRequestIndex != null) {
        intRequestIndex = Integer.parseInt(strRequestIndex);
    }

    if (intRequestIndex < 10) {
        try {
            callSameServlet(intRequestIndex);
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}

private void callSameServlet(int requestIndex) throws Exception {

    URL recursiveUrl = new URL(request.getRequestURL().toString()
            + "?requestIndex=" + (requestIndex + 1));

    HttpURLConnection connection = (HttpURLConnection) recursiveUrl
            .openConnection();

    connection.setRequestMethod("POST");
    connection.setDoInput(true);
    connection.setDoOutput(true);
    connection.setUseCaches(false);
    connection.connect();

    DataOutputStream dos = new DataOutputStream(
            connection.getOutputStream());

    dos.flush();
    dos.close();

}

}
