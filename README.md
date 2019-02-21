# Bevezerés az osztott rendszerekbe

**Kliens**
````JAVA

import java.io.*;
import java.util.*;
import java.net.*;

class ElsoKliens {
	public static void main(String[] args) throws Exception {
		String MACHINE = "localhost";
		// String MACHINE = "127.0.0.1";
		// String MACHINE = "::1";
		int PORT = 12345;
		try (
			Socket s = new Socket(MACHINE, PORT);
			Scanner sc = new Scanner(s.getInputStream());
			PrintWriter pw = new PrintWriter(s.getOutputStream());
		) {
			pw.println("AzEnNevem");
			pw.flush(); //atkuldi az utolsó információt a szerverről

			String valasz = sc.nextLine();
			//sc.hasNextLine 	//várja következő sort vagy a halott servert
			//sc.hasNext 		//átugorja az össes whitespacet
			//sc.hasNextInt 	//átugorja a whitespaceket és számot vár válaszként

			System.out.println(valasz);
		}
	}
}
````

**Server**
````JAVA

import java.io.*;
import java.util.*;
import java.net.*;

class ElsoSzerver {
	public static void main(String[] args) throws Exception {
		// protokoll
			// interfesz
		// 0-65535 (1024-65535)
		int PORT = 12345;
		// try-with-resources (AutoCloseable)
		try (
			ServerSocket ss = new ServerSocket(PORT);
			Socket s = ss.accept(); // blokkoló hívás, addig vár amíg egy kliens meg nem jelenik, akár évekig...
			Scanner sc = new Scanner(s.getInputStream());
			PrintWriter pw = new PrintWriter(s.getOutputStream());
		) {//erőforrás kezelő try blokk
			String name = sc.nextLine();

			pw.println("Hello, " + name);
			pw.flush();
		} //itt szabadíja fel a lefogallt erőforrásokat
	}
}
````

## Tesztelni
Tesztelni Puttyal lehet, a `localhoston` az `12345` porton `Raw` típusú kapcsolatban, és ne lépjen ki ha szakad a kapcsolat mert akkor nem látunk semmit.
