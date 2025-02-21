package com.medicaldelivery.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Product {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private double price;

    public Product() {}

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }
 public Long getId() { return id; }
 public void setId(Long id) { this.id = id; }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public double getPrice() { return price; }
    public void setPrice(double price) { this.price = price; }
}
![image](https://github.com/user-attachments/assets/169d0776-112a-4cd4-864c-08eb8813b0bf)
package com.medicaldelivery.model;

import javax.persistence.*;

@Entity
public class CartItem {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    private Product product;
    private int quantity;

public CartItem() {}

    public CartItem(Product product, int quantity) {
        this.product = product;
        this.quantity = quantity;
    }

    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }

    public Product getProduct() { return product; }
    public void setProduct(Product product) { this.product = product; }

    public int getQuantity() { return quantity; }
    public void setQuantity(int quantity) { this.quantity = quantity; }
}
![image](https://github.com/user-attachments/assets/772f8b6e-8c63-41fd-b767-c67a8234ec25)
package com.medicaldelivery.controller;

import com.medicaldelivery.model.Product;
import com.medicaldelivery.service.ProductService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/products")
public class ProductController {

    @Autowired
    private ProductService productService;

    @GetMapping
    public List<Product> getAllProducts() {
        return productService.getAllProducts();
    }
}
![image](https://github.com/user-attachments/assets/643559cf-a55a-4a84-b8bd-3835ba3c9aa5)
package com.medicaldelivery.service;

import com.medicaldelivery.model.CartItem;
import com.medicaldelivery.model.Product;
import com.medicaldelivery.repository.CartItemRepository;
import com.medicaldelivery.repository.ProductRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class CartService {

    @Autowired
    private CartItemRepository cartItemRepository;
    @Autowired
    private ProductRepository productRepository;

    public List<CartItem> getCartItems() {
        return cartItemRepository.findAll();
    }

    public CartItem addToCart(Long productId, int quantity) {
        Product product = productRepository.findById(productId)
                            .orElseThrow(() -> new RuntimeException("Product not found"));
        CartItem cartItem = new CartItem(product, quantity);
        return cartItemRepository.save(cartItem);
    }

    public void checkout() {
        cartItemRepository.deleteAll();
    }
}
![image](https://github.com/user-attachments/assets/15059e36-23cf-4bed-ba81-bb6f0243abca)




