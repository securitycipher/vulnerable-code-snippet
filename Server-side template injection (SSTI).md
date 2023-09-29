Here is an example of vulnerable code that is susceptible to a Server-side template injection (SSTI) Vulnerability :

## 🥺 Vulnerable Code
```
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.stereotype.Controller;

@Controller
public class TemplateController {

@GetMapping("/greet")
public String greet(@RequestParam(value = "name", defaultValue = "Guest") String name, Model model) {
// Vulnerable code: Embeds user input directly into a template
model.addAttribute("message", "Hello, " + name + "!");
return "greeting";
}
}
```
In the vulnerable code snippet above, user input (name) is directly concatenated into the template without proper validation or sanitization. An attacker could potentially exploit this by injecting malicious template code, leading to SSTI.

## 😎 Secure Code
```
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.stereotype.Controller;
import org.owasp.encoder.Encode;

@Controller
public class TemplateController {

@GetMapping("/greet")
public String greet(@RequestParam(value = "name", defaultValue = "Guest") String name, Model model) {
// Secure code: Properly encode user input to prevent SSTI
model.addAttribute("message", "Hello, " + Encode.forHtml(name) + "!");
return "greeting";
}
}
```
In the secure code snippet, we use the OWASP Java Encoder (Encode.forHtml) to properly encode the user input before embedding it into the template. This prevents any malicious template code from being executed and mitigates the risk of SSTI.
