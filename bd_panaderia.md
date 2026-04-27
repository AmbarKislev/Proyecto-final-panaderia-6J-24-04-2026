Claro, te doy un ejemplo claro de cómo modelar una **base de datos documental (tipo NoSQL, como MongoDB)** para una panadería. Aquí no usas tablas, sino **colecciones** y **documentos JSON**.

---

# 📦 Colecciones principales para una panadería

## 1. 🧾 Colección: `productos`

Contiene todos los productos que vende la panadería.

### Ejemplo de documento:

```json
{
  "_id": "prod001",
  "nombre": "Concha de vainilla",
  "categoria": "pan dulce",
  "precio": 12.50,
  "costo": 5.00,
  "disponible": true,
  "ingredientes": [
    "harina",
    "azúcar",
    "huevo",
    "mantequilla",
    "vainilla"
  ],
  "fecha_creacion": "2026-04-20T10:30:00Z"
}
```

### Atributos y tipos:

* `_id`: string
* `nombre`: string
* `categoria`: string
* `precio`: number (decimal)
* `costo`: number (decimal)
* `disponible`: boolean
* `ingredientes`: array<string>
* `fecha_creacion`: date

---

## 2. 👤 Colección: `clientes`

Información de los clientes.

### Ejemplo:

```json
{
  "_id": "cli001",
  "nombre": "Juan Pérez",
  "telefono": "6561234567",
  "email": "juan@example.com",
  "direccion": {
    "calle": "Av. Central",
    "numero": "123",
    "colonia": "Centro",
    "ciudad": "Juárez"
  },
  "fecha_registro": "2026-04-01T09:00:00Z"
}
```

### Tipos:

* `_id`: string
* `nombre`: string
* `telefono`: string
* `email`: string
* `direccion`: object
* `fecha_registro`: date

---

## 3. 🛒 Colección: `ventas`

Registra cada venta realizada.

### Ejemplo:

```json
{
  "_id": "venta001",
  "cliente_id": "cli001",
  "fecha": "2026-04-27T08:45:00Z",
  "productos": [
    {
      "producto_id": "prod001",
      "nombre": "Concha de vainilla",
      "cantidad": 3,
      "precio_unitario": 12.50,
      "subtotal": 37.50
    }
  ],
  "total": 37.50,
  "metodo_pago": "efectivo"
}
```

### Tipos:

* `_id`: string
* `cliente_id`: string
* `fecha`: date
* `productos`: array<object>
* `total`: number
* `metodo_pago`: string

---

## 4. 📦 Colección: `inventario`

Control de materias primas.

### Ejemplo:

```json
{
  "_id": "inv001",
  "nombre": "Harina",
  "cantidad": 50,
  "unidad": "kg",
  "stock_minimo": 10,
  "proveedor": "Proveedor ABC",
  "ultima_actualizacion": "2026-04-25T12:00:00Z"
}
```

### Tipos:

* `_id`: string
* `nombre`: string
* `cantidad`: number
* `unidad`: string
* `stock_minimo`: number
* `proveedor`: string
* `ultima_actualizacion`: date

---

## 5. 🚚 Colección: `proveedores`

Información de proveedores.

### Ejemplo:

```json
{
  "_id": "prov001",
  "nombre": "Proveedor ABC",
  "telefono": "6569876543",
  "email": "contacto@proveedor.com",
  "direccion": "Zona Industrial, Juárez",
  "productos_suministrados": ["harina", "azúcar"]
}
```

---

# 🔗 Relaciones (en bases documentales)

En NoSQL puedes usar:

* **Referencias**:

  * `cliente_id` en ventas
  * `producto_id` en ventas

* **Documentos embebidos**:

  * productos dentro de ventas
  * dirección dentro de cliente

---

# 💡 Buenas prácticas

* Usa **datos embebidos** cuando se consultan juntos (ventas + productos)
* Usa **referencias** cuando los datos crecen mucho (clientes, productos)
* Mantén consistencia en nombres de campos
* Usa fechas en formato ISO (`YYYY-MM-DDTHH:MM:SSZ`)

---

Si quieres, puedo darte:
✅ el diseño listo para MongoDB con comandos `insertOne()`
✅ un diagrama visual tipo ER adaptado a NoSQL
✅ o cómo conectarlo con una app (Node.js, Python, etc.)
