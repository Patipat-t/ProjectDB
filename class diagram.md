# Class Diagram - Hondana Manga & Light Novel Hub

เอกสารนี้แสดงโครงสร้าง Class Diagram ที่ถูกสกัดและออกแบบจากโค้ด `app.py` ทั้งในส่วนของ **Database Entities (3NF Data Model)** และ **Backend Application Infrastructure (HTTP Server & Handlers)**

---

```mermaid
classDiagram
    %% ==========================================
    %% 1. SYSTEM & HTTP INFRASTRUCTURE CLASSES
    %% ==========================================
    class AppRequestHandler {
        +do_GET() void
        +do_POST() void
        +do_PUT() void
        +do_PATCH() void
        +do_DELETE() void
        +do_OPTIONS() void
        +send_json_response(data: dict, status: int) void
        +send_error_response(message: str, status: int) void
        +get_request_body() dict
        +get_current_user_id() int
        +is_current_user_admin(conn: Connection) bool
    }

    class DatabaseManager {
        <<Utility>>
        +DB_FILE: str
        +STATIC_DIR: str
        +PORT: int
        +get_db() Connection
        +init_db() void
        +seed_mock_data(conn: Connection) void
        +run_server() void
    }

    %% ==========================================
    %% 2. DOMAIN MODEL / DATABASE ENTITIES (3NF)
    %% ==========================================
    class Role {
        +role_id: int [PK]
        +role_name: string [UNIQUE]
    }

    class User {
        +user_id: int [PK]
        +email: string [UNIQUE]
        +password_hash: string
        +full_name: string
        +phone: string
        +pdpa_consent: int
        +pdpa_consent_date: datetime
        +avatar_url: string
        +role_id: int [FK]
        +created_at: datetime
    }

    class Category {
        +category_id: int [PK]
        +category_name: string [UNIQUE]
    }

    class Author {
        +author_id: int [PK]
        +author_name: string
        +bio: string
    }

    class Ebook {
        +ebook_id: int [PK]
        +title: string
        +description: string
        +price: float
        +cover_image_url: string
        +is_active: int
        +category_id: int [FK]
        +author_id: int [FK]
    }

    class Cart {
        +cart_id: int [PK]
        +user_id: int [FK, UNIQUE]
        +updated_at: datetime
    }

    class CartItem {
        +cart_item_id: int [PK]
        +cart_id: int [FK]
        +ebook_id: int [FK]
        +quantity: int
    }

    class Order {
        +order_id: int [PK]
        +order_code: string [UNIQUE]
        +user_id: int [FK]
        +order_date: datetime
        +total_amount: float
        +status: string
    }

    class OrderItem {
        +order_item_id: int [PK]
        +order_id: int [FK]
        +ebook_id: int [FK]
        +quantity: int
        +unit_price: float
    }

    class Payment {
        +payment_id: int [PK]
        +order_id: int [FK, UNIQUE]
        +payment_method: string
        +proof_image: string
        +status: string
        +paid_at: datetime
    }

    class DownloadLink {
        +download_id: int [PK]
        +order_id: int [FK]
        +ebook_id: int [FK]
        +file_name: string
        +file_size: string
        +download_url: string
        +expires_at: datetime
    }

    %% ==========================================
    %% 3. RELATIONSHIPS & MULTIPLICITIES
    %% ==========================================
    
    %% Infrastructure Interactions
    AppRequestHandler ..> DatabaseManager : Uses get_db()
    
    %% Domain Entity Relationships
    Role "1" -- "0..*" User : assigns
    User "1" -- "0..1" Cart : owns
    User "1" -- "0..*" Order : places
    
    Category "1" -- "0..*" Ebook : classifies
    Author "1" -- "0..*" Ebook : writes
    
    Cart "1" -- "0..*" CartItem : contains
    Ebook "1" -- "0..*" CartItem : listed_in
    
    Order "1" -- "1..*" OrderItem : consists_of
    Ebook "1" -- "0..*" OrderItem : referenced_in
    
    Order "1" -- "0..1" Payment : has
    Order "1" -- "0..*" DownloadLink : generates
    Ebook "1" -- "0..*" DownloadLink : grants_access
