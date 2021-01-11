# CRUD in Spring Boot

### Step 1 - pom.xml

Add these to the `<dependencies></dependencies>` tag in the file and save it

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

### Step 2 - Connecting to MySQL
* In the command line `mysql -u root -p`
* create a schema for the project `create schema coffeeshop;`

Next add this code in `src/main/resources/application.properties`

```
spring.datasource.url=jdbc:mysql://localhost:3306/<SCHEMA_NAME>?useUnicode=yes&characterEncoding=UTF-8&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.jpa.hibernate.ddl-auto=update
spring.mvc.hiddenmethod.filter.enabled=true

```
### Step 3 - Domain Model With Validation

In pom.xml add these to the `<dependencies></dependencies>` tag in the file and save it

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```
* Create a new package `/src/main/java/com.yourname.coffeeshop.models`
* Inside it create a new `Java Class` called example `Shop`

Next create your model with validation for example

```java
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.validation.constraints.NotEmpty;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;

@Entity
@Table(name="Shops")
public class Shop {
	
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;

    @Size(min=3, message="Coffee Shop name must be at least 3 characters!")
	private String name;

    @Size(max=50, message="Coffee Shop location must be less than 30 characters!")
	private String location;

    @DateTimeFormat(pattern="yyyy-MM-dd")
	@NotNull(message="Founded On date is required!")
    private Date founded;

    @NotEmpty(message="Description is required!")
	private String description;
	
    public Shop() {}

    public String toString() {
		String res = "";
		res += "Name: " + name + "\n";
		res += "Location: " + location + "\n";
		res += "Founded: " + founded + "\n";
		res += "Description: " + description + "\n";
		return res;
	}
	
}

```
Finally Set the getters and setters

### Step 4 - Data Repository
* Create a new package `/src/main/java/com.yourname.coffeeshop.repositories`
* Inside it create a new `Java Interface` called example `ShopRepository`

add this code in it

```java
import org.springframework.data.repository.CrudRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface ShopRepository extends CrudRepository<Shop, Long> {}

```
### Step 5 -  Services
* Create a new package `/src/main/java/com.yourname.coffeeshop.services`
* Inside it create a new `Java class` called example `ShopService`

add this code in it

```java
@Service
public class ShopService {

	private ShopRepository shopRepo;
	
	public ShopService(ShopRepository shopRepo) {
		this.shopRepo = shopRepo;
	}
	
	public Shop create(Shop newShop) {
		return shopRepo.save(newShop);
	}
	
	public ArrayList<Shop> getAll() {
		return (ArrayList<Shop>) shopRepo.findAll();
	}
	
	public Shop getOne(Long id) {
		Optional<Shop> shop = shopRepo.findById(id);
		return shop.get();
	}
	
	public Shop update(Shop toEdit) {
		return shopRepo.save(toEdit);
	}
	
	public void remove(Long id) {
		shopRepo.deleteById(id);
	}
}

```
## Setting the CRUD operations

open the file `/src/main/java/com.yourname.coffeeshop.controllers/HomeController`
```java
@Controller
public class HomeController {
	
	private ShopService shopServ;
	
	public HomeController(ShopService shopServ) {
		this.shopServ = shopServ;
	}
	//Print All
    @GetMapping("/")
    public String index(Model model) {
    	model.addAttribute("shop", new Shop());
    	model.addAttribute("shops", shopServ.getAll());
        return "index.jsp";
    }
    //Post
    @PostMapping("/shop")
    public String create(@Valid @ModelAttribute("shop") Shop shop, BindingResult result, 
    		HttpSession session, Model model) {
    	if(result.hasErrors()) {
    		model.addAttribute("shops", shopServ.getAll());
    		return "index.jsp"; 
    	}
    	session.setAttribute("shop", shop);
    	shopServ.create(shop);
    	return "redirect:/shop"; 
    }
    //Show one
    @GetMapping("/shop/{id}")
    public String showShop(@PathVariable("id") Long id, Model model) {
		model.addAttribute("shop", shopServ.getOne(id));
    	return "shop.jsp";
    }
    //Update
    @GetMapping("/shop/{id}/edit")
    public String edit(@PathVariable("id") Long id, Model model) {
    	model.addAttribute("toEdit", shopServ.getOne(id));
    	return "edit.jsp";
    }
    @PostMapping("/shop/{id}/update")
    public String edit(@Valid @ModelAttribute("toEdit") Shop toEdit, BindingResult result, 
    		@PathVariable("id") Long id, Model model) {
    	if(result.hasErrors()) {return "edit.jsp";}
    	shopServ.update(toEdit);
    	return "redirect:/shop/" + id;
    }
    //Delete
    @GetMapping("/shop/{id}/delete")
    public String delete(@PathVariable("id") Long id) {
    	shopServ.remove(id);
    	return "redirect:/";
    }
}

