# 📋 APIs del Sistema E-commerce

## 🎯 Información General

- **Base URL**: `http://localhost:3001/api`
- **Autenticación**: Bearer Token (JWT)
- **Formato**: JSON
- **Códigos de respuesta estándar**: 200 (OK), 201 (Created), 400 (Bad Request), 401 (Unauthorized), 403 (Forbidden), 404 (Not Found), 500 (Internal Server Error)

---

## 🔐 **Autenticación**

### `POST /auth/login`
**RF-01**: Autenticación por DNI y contraseña

**Request:**
```json
{
  "dni": "12345678",
  "password": "contraseña123"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": 1,
      "dni": "12345678",
      "fullName": "Juan Pérez",
      "email": "juan@email.com",
      "role": "afiliado",
      "isActive": true
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expiresIn": "7d"
  }
}
```

### `POST /auth/register`
**RF-02**: Registro de nuevo usuario

**Request:**
```json
{
  "dni": "87654321",
  "fullName": "María García",
  "email": "maria@email.com",
  "password": "contraseña123",
  "role": "afiliado",
  "phone": "987654321",
  "region": "Lima",
  "city": "San Isidro",
  "address": "Av. Principal 123",
  "reference": "Frente al parque",
  "sponsorId": 1
}
```

### `POST /auth/logout`
**Request:** Bearer Token required

### `GET /auth/me`
Obtener información del usuario autenticado

---

## 👥 **Usuarios**

### `GET /users`
Listar usuarios (solo admins)

**Query Params:**
- `page`: número de página (default: 1)
- `limit`: items por página (default: 10)
- `role`: filtrar por rol
- `search`: buscar por nombre o DNI

### `GET /users/:id`
Obtener usuario específico

### `PUT /users/:id`
Actualizar usuario

### `DELETE /users/:id`
Desactivar usuario

### `PUT /users/:id/activate`
**RF-05**: Activar/desactivar cuenta por compra mínima

---

## 🏷️ **Categorías**

### `GET /categories`
**RF-03**: Listar categorías activas

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Suplementos",
      "description": "Suplementos nutricionales",
      "isActive": true,
      "createdAt": "2025-07-11T10:00:00Z"
    }
  ]
}
```

### `POST /categories`
Crear categoría (admin)

### `PUT /categories/:id`
Actualizar categoría

### `DELETE /categories/:id`
Eliminar categoría

---

## 📦 **Productos**

### `GET /products`
**RF-03**: Catálogo de productos

**Query Params:**
- `category`: ID de categoría
- `search`: buscar por nombre
- `page`: número de página
- `limit`: items por página
- `userRole`: para mostrar precios correctos

**Response:**
```json
{
  "success": true,
  "data": {
    "products": [
      {
        "id": 1,
        "name": "Proteína Whey 2kg",
        "description": "Proteína de alta calidad",
        "price": 120.00,
        "affiliatePrice": 100.00,
        "stock": 50,
        "isActive": true,
        "imageUrl": "/images/protein.jpg",
        "category": {
          "id": 1,
          "name": "Suplementos"
        }
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 25,
      "pages": 3
    }
  }
}
```

### `GET /products/:id`
Detalle de producto específico

### `POST /products`
Crear producto (admin)

### `PUT /products/:id`
Actualizar producto

### `DELETE /products/:id`
Eliminar producto

### `PUT /products/:id/stock`
Actualizar stock

---

## 🛒 **Carrito**

### `GET /cart`
**RF-04**: Obtener carrito del usuario

**Response:**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "items": [
      {
        "id": 1,
        "product": {
          "id": 1,
          "name": "Proteína Whey 2kg",
          "price": 120.00,
          "affiliatePrice": 100.00,
          "imageUrl": "/images/protein.jpg"
        },
        "quantity": 2
      }
    ],
    "totalItems": 2,
    "totalPrice": 200.00
  }
}
```

### `POST /cart/items`
Agregar item al carrito

**Request:**
```json
{
  "productId": 1,
  "quantity": 2
}
```

### `PUT /cart/items/:itemId`
Actualizar cantidad

### `DELETE /cart/items/:itemId`
Eliminar item del carrito

### `DELETE /cart`
Vaciar carrito

---

## 📋 **Pedidos**

### `GET /orders`
**RF-09**: Mis pedidos (historial filtrable)

**Query Params:**
- `status`: filtrar por estado
- `dateFrom`: fecha desde
- `dateTo`: fecha hasta
- `page`: número de página

**Response:**
```json
{
  "success": true,
  "data": {
    "orders": [
      {
        "id": 1,
        "status": "paid",
        "totalAmount": 200.00,
        "shippingCost": 15.00,
        "createdAt": "2025-07-11T10:00:00Z",
        "trackingCode": "SH123456",
        "orderItems": [
          {
            "id": 1,
            "product": {
              "name": "Proteína Whey 2kg"
            },
            "quantity": 2,
            "unitPrice": 100.00
          }
        ]
      }
    ]
  }
}
```

### `GET /orders/:id`
Detalle de pedido específico

### `POST /orders`
**RF-04**: Crear pedido (checkout)

**Request:**
```json
{
  "shippingAddress": {
    "name": "Juan Pérez",
    "phone": "987654321",
    "region": "Lima",
    "city": "San Isidro",
    "address": "Av. Principal 123",
    "reference": "Frente al parque"
  },
  "paymentMethod": "BCP_code",
  "bcpCode": "BCP123456789"
}
```

### `PUT /orders/:id/status`
**RF-13**: Actualizar estado de pedido (admin)

**Request:**
```json
{
  "status": "shipped",
  "trackingCode": "SH123456",
  "shalomAgency": "Shalom San Isidro",
  "shalomGuide": "GU123456"
}
```

---

## 💳 **Pagos**

### `POST /payments/validate-bcp`
**RF-04**: Validar código BCP

**Request:**
```json
{
  "bcpCode": "BCP123456789",
  "amount": 215.00,
  "orderId": 1
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "valid": true,
    "amount": 215.00,
    "reference": "REF123456",
    "paidAt": "2025-07-11T10:30:00Z"
  }
}
```

### `GET /payments/:orderId`
Obtener pagos de un pedido

---

## 💰 **Comisiones**

### `GET /commissions/my-commissions`
**RF-07**: Panel "Mis Comisiones"

**Query Params:**
- `month`: mes específico (YYYY-MM)
- `type`: direct, referral
- `status`: pending, approved, paid

**Response:**
```json
{
  "success": true,
  "data": {
    "summary": {
      "currentMonth": {
        "total": 250.00,
        "direct": 150.00,
        "referral": 100.00
      },
      "allTime": {
        "total": 1250.00,
        "direct": 750.00,
        "referral": 500.00
      }
    },
    "commissions": [
      {
        "id": 1,
        "type": "direct",
        "amount": 20.00,
        "percentage": 20.00,
        "status": "approved",
        "createdAt": "2025-07-11T10:00:00Z",
        "orderItem": {
          "product": {
            "name": "Proteína Whey 2kg"
          },
          "quantity": 1
        }
      }
    ]
  }
}
```

### `GET /commissions/by-referral`
**RF-07**: Desglose por referido

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "referral": {
        "id": 2,
        "fullName": "María García",
        "dni": "87654321"
      },
      "totalCommissions": 150.00,
      "monthlyCommissions": 50.00,
      "totalOrders": 5
    }
  ]
}
```

### `PUT /commissions/:id/approve`
**RF-13**: Aprobar comisión (admin)

### `GET /commissions/pending`
**RF-13**: Comisiones pendientes (admin)

---

## 👥 **Red de Afiliados**

### `GET /affiliates/my-network`
**RF-08**: Panel "Mi red de afiliados"

**Response:**
```json
{
  "success": true,
  "data": {
    "summary": {
      "totalAffiliates": 15,
      "activeAffiliates": 12,
      "totalCommissionsGenerated": 500.00,
      "monthlyCommissionsGenerated": 150.00
    },
    "affiliates": [
      {
        "id": 2,
        "fullName": "María García",
        "dni": "87654321",
        "phone": "987654321",
        "city": "San Isidro",
        "status": "active",
        "stats": {
          "totalOrders": 8,
          "monthlyOrders": 2,
          "totalSpent": 800.00,
          "monthlySpent": 200.00,
          "commissionsGenerated": 80.00,
          "monthlyCommissionsGenerated": 20.00
        },
        "referredAt": "2025-06-01T10:00:00Z"
      }
    ]
  }
}
```

### `POST /affiliates/register-referral`
**RF-02**: Registrar nuevo afiliado

**Request:**
```json
{
  "dni": "11223344",
  "fullName": "Carlos López",
  "email": "carlos@email.com",
  "phone": "987654321",
  "region": "Lima",
  "city": "Miraflores",
  "address": "Av. Secundaria 456",
  "reference": "Cerca del mall"
}
```

### `GET /affiliates/:id/stats`
Estadísticas de un afiliado específico

---

## 🏆 **Premios**

### `GET /rewards`
**RF-10**: Catálogo de premios

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Smartphone Samsung",
      "description": "Galaxy A54 128GB",
      "pointsRequired": 1000,
      "imageUrl": "/images/samsung.jpg",
      "stock": 5,
      "isActive": true
    }
  ]
}
```

### `GET /rewards/my-points`
**RF-10**: Puntos del afiliado

**Response:**
```json
{
  "success": true,
  "data": {
    "currentPoints": 750,
    "totalEarned": 1200,
    "totalSpent": 450,
    "availableRewards": [
      {
        "id": 2,
        "name": "Audífonos Bluetooth",
        "pointsRequired": 500
      }
    ]
  }
}
```

### `POST /rewards/claim`
**RF-10**: Canjear premio

**Request:**
```json
{
  "rewardId": 2
}
```

### `GET /rewards/my-claims`
Historial de canjes

---

## 📚 **Tutoriales**

### `GET /tutorials`
**RF-11**: Centro de capacitación

**Query Params:**
- `category`: sales, platform, products
- `contentType`: video, pdf

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "title": "Cómo registrar un afiliado",
      "description": "Paso a paso para registrar nuevos afiliados",
      "url": "https://youtube.com/watch?v=abc123",
      "contentType": "video",
      "category": "sales",
      "order": 1
    }
  ]
}
```

### `GET /tutorials/:id`
Detalle de tutorial específico

---

## 🔔 **Notificaciones**

### `GET /notifications`
**RF-12**: Notificaciones del usuario

**Query Params:**
- `unread`: true/false
- `type`: payment_ok, commission, account_pending

**Response:**
```json
{
  "success": true,
  "data": {
    "unreadCount": 3,
    "notifications": [
      {
        "id": 1,
        "type": "payment_ok",
        "title": "Pago confirmado",
        "message": "Tu pedido #123 ha sido pagado exitosamente",
        "readFlag": false,
        "createdAt": "2025-07-11T10:00:00Z"
      }
    ]
  }
}
```

### `PUT /notifications/:id/read`
Marcar notificación como leída

### `PUT /notifications/mark-all-read`
Marcar todas como leídas

---

## 🏠 **Direcciones**

### `GET /addresses`
Listar direcciones del usuario

### `POST /addresses`
Crear nueva dirección

### `PUT /addresses/:id`
Actualizar dirección

### `DELETE /addresses/:id`
Eliminar dirección

### `PUT /addresses/:id/set-default`
Establecer como dirección por defecto

---

## 📊 **Administración**

### `GET /admin/dashboard`
**RF-13, RF-14**: Dashboard administrativo

**Response:**
```json
{
  "success": true,
  "data": {
    "kpis": {
      "totalSales": 50000.00,
      "monthlyOrders": 150,
      "activeAffiliates": 85,
      "pendingCommissions": 2500.00
    },
    "recentOrders": [...],
    "lowStockProducts": [...],
    "pendingCommissions": [...]
  }
}
```

### `GET /admin/orders`
**RF-13**: Gestión de pedidos

**Query Params:**
- `status`: pending, paid, shipped, delivered
- `region`: filtrar por región (solo admins regionales)

### `GET /admin/users`
**RF-13**: Gestión de usuarios

### `GET /admin/commissions/pending`
**RF-13**: Comisiones pendientes de aprobación

---

## ⚙️ **Configuración**

### `GET /config/business-rules`
**RF-14**: Obtener reglas de negocio

**Response:**
```json
{
  "success": true,
  "data": {
    "minMonthlyBuy": 1,
    "referralCommissionPercentage": 10,
    "directSaleCommissionPercentage": 20,
    "shippingCost": 15.00
  }
}
```

### `PUT /config/business-rules`
**RF-14**: Actualizar reglas de negocio (admin general)

### `GET /config/regions`
Listar regiones disponibles

---

## 📈 **Reportes**

### `GET /reports/sales`
**RF-14**: Reporte de ventas

**Query Params:**
- `dateFrom`, `dateTo`: rango de fechas
- `region`: filtrar por región
- `groupBy`: day, week, month

### `GET /reports/commissions`
**RF-14**: Reporte de comisiones

### `GET /reports/affiliates`
**RF-14**: Reporte de afiliados

---

## 🔍 **Búsqueda**

### `GET /search`
Búsqueda global

**Query Params:**
- `q`: término de búsqueda
- `type`: products, users, orders

---

## 📱 **Webhooks** (Futuro)

### `POST /webhooks/bcp`
**RF-04**: Webhook para confirmación automática de pagos BCP

### `POST /webhooks/shalom`
**RF-15**: Webhook para actualizaciones de seguimiento Shalom

---

## 🛡️ **Middleware de Autenticación**

### Rutas Públicas:
- `GET /products`
- `GET /categories`
- `POST /auth/login`
- `POST /auth/register`

### Rutas de Usuario Autenticado:
- Todas las rutas con prefijo `/cart`, `/orders`, `/commissions`, `/affiliates`, `/notifications`

### Rutas de Administrador:
- Todas las rutas con prefijo `/admin`
- `POST /products`, `PUT /products/:id`, `DELETE /products/:id`
- `PUT /config/business-rules`

### Rutas de Administrador General:
- `GET /admin/dashboard` (datos globales)
- `PUT /config/business-rules`
- Gestión de reglas y premios

---

