# TRANSPORTE VIP - Aplicación Web de Reservas

## 🚗 Descripción

Aplicación web profesional bilingüe (Español/Inglés) para servicios de transporte VIP en Panamá. Incluye cotización de viajes, reservas, generación automática de facturas PDF y sistema de notificaciones por correo electrónico.

## ✨ Características

- **Bilingüe**: Español e Inglés con cambio instantáneo
- **Cotización de Viajes**: Estimación automática de precios
- **Reservas Online**: Formulario completo con validación
- **Generación de Facturas PDF**: Automática con información completa
- **Múltiples Servicios**: Tours, traslados aeropuerto, servicio corporativo
- **Métodos de Pago**: Efectivo, Yappy, Tarjeta, Clave
- **Facturación Fiscal**: Opción para solicitar factura con RUC
- **Diseño Profesional**: Interfaz moderna y elegante
- **Responsive**: Funciona en móviles, tablets y desktop

## 🚀 Implementación Rápida (GRATIS)

### Opción 1: GitHub Pages (Recomendado)

#### Paso 1: Crear cuenta en GitHub
1. Ve a [github.com](https://github.com)
2. Click en "Sign up" (es gratis)
3. Completa el registro

#### Paso 2: Subir tu proyecto
1. Click en el botón "+" arriba a la derecha
2. Selecciona "New repository"
3. Nombre: `transporte-vip`
4. Marca "Public"
5. Click "Create repository"

#### Paso 3: Subir el archivo
1. En la página del repositorio, click "uploading an existing file"
2. Arrastra el archivo `transporte-vip.html`
3. Click "Commit changes"

#### Paso 4: Activar GitHub Pages
1. Ve a "Settings" en tu repositorio
2. En el menú lateral, click "Pages"
3. En "Source", selecciona "main" branch
4. Click "Save"
5. ¡Tu sitio estará en: `https://TU-USUARIO.github.io/transporte-vip/transporte-vip.html`

### Opción 2: Netlify (Más fácil, con dominio personalizado)

1. Ve a [netlify.com](https://www.netlify.com)
2. Click "Sign up" (gratis)
3. Arrastra tu archivo HTML a la zona de "Drop"
4. ¡Listo! Tu sitio está en línea
5. Netlify te dará una URL como: `https://random-name.netlify.app`
6. Puedes cambiar el nombre en Site Settings

### Opción 3: Vercel (Rápido y profesional)

1. Ve a [vercel.com](https://vercel.com)
2. Click "Sign up" (gratis)
3. Conecta con GitHub
4. Importa tu repositorio
5. Deploy automático

## 📧 Configuración de Correo Electrónico (EmailJS - GRATIS)

### Paso 1: Crear cuenta en EmailJS
1. Ve a [emailjs.com](https://www.emailjs.com)
2. Click "Sign Up" (100 emails/mes gratis)
3. Verifica tu email

### Paso 2: Configurar servicio de email
1. En el dashboard, click "Add New Service"
2. Selecciona tu proveedor (Gmail recomendado)
3. Conecta tu cuenta de Gmail
4. Copia el "Service ID"

### Paso 3: Crear plantilla de email
1. Click "Email Templates"
2. Click "Create New Template"
3. Diseña tu plantilla con estas variables:
   ```
   {{firstName}} {{lastName}}
   {{phone}}
   {{email}}
   {{pickupLocation}}
   {{destination}}
   {{tripDate}}
   {{tripTime}}
   {{paymentMethod}}
   {{estimatedPrice}}
   ```
4. Copia el "Template ID"

### Paso 4: Integrar en el código
1. Abre `transporte-vip.html`
2. Busca la línea: `// In a real application, you would send this via email`
3. Reemplaza la función `generateInvoice` al final con este código:

```javascript
// Inicializar EmailJS (agrega después de cargar la librería)
emailjs.init("TU_USER_ID"); // Lo encuentras en Account > API Keys

function generateInvoice(data) {
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();
    
    // ... [mantén todo el código de generación del PDF] ...
    
    // Convertir PDF a base64
    const pdfBase64 = doc.output('datauristring');
    
    // Preparar datos para el email
    const emailData = {
        to_email: data.email,
        to_name: `${data.firstName} ${data.lastName}`,
        firstName: data.firstName,
        lastName: data.lastName,
        phone: data.phone,
        email: data.email,
        pickupLocation: data.pickupLocation,
        destination: data.destination,
        tripDate: data.tripDate,
        tripTime: data.tripTime,
        paymentMethod: data.paymentMethod,
        estimatedPrice: data.estimatedPrice.toFixed(2),
        driverName: "Juan Carlos Rodríguez",
        driverPhone: "+507 6123-4567",
        driverLicense: "8-123-4567",
        vehicleInfo: "Toyota Camry 2023 - Negro",
        needsInvoice: data.needsInvoice ? 'Sí' : 'No',
        pdf_attachment: pdfBase64
    };
    
    // Enviar email al cliente
    emailjs.send('TU_SERVICE_ID', 'TU_TEMPLATE_ID', emailData)
        .then(function(response) {
            console.log('Email enviado!', response);
            
            // Enviar copia al conductor
            const driverEmail = {
                ...emailData,
                to_email: "conductor@transportevip.com" // Email del conductor
            };
            
            emailjs.send('TU_SERVICE_ID', 'TU_TEMPLATE_ID_CONDUCTOR', driverEmail);
            
        }, function(error) {
            console.log('Error al enviar email:', error);
        });
    
    // Guardar PDF
    doc.save(`Factura_TransporteVIP_${data.firstName}_${data.lastName}.pdf`);
}
```

### Plantilla de Email Sugerida (EmailJS)

**Asunto:** Confirmación de Reserva - Transporte VIP

**Cuerpo:**
```html
<h2>¡Reserva Confirmada!</h2>

<p>Estimado/a {{firstName}} {{lastName}},</p>

<p>Su viaje ha sido agendado exitosamente. Aquí están los detalles:</p>

<h3>Información del Viaje</h3>
<ul>
    <li><strong>Recogida:</strong> {{pickupLocation}}</li>
    <li><strong>Destino:</strong> {{destination}}</li>
    <li><strong>Fecha:</strong> {{tripDate}}</li>
    <li><strong>Hora:</strong> {{tripTime}}</li>
    <li><strong>Costo Estimado:</strong> ${{estimatedPrice}}</li>
    <li><strong>Método de Pago:</strong> {{paymentMethod}}</li>
</ul>

<h3>Información del Conductor</h3>
<ul>
    <li><strong>Nombre:</strong> {{driverName}}</li>
    <li><strong>Teléfono:</strong> {{driverPhone}}</li>
    <li><strong>Cédula:</strong> {{driverLicense}}</li>
    <li><strong>Vehículo:</strong> {{vehicleInfo}}</li>
</ul>

<p><em>* El precio final puede variar debido a tiempo de espera, tráfico u otros factores.</em></p>
<p><em>* El pago se realiza al finalizar el viaje.</em></p>

<p>Adjunto encontrará su factura informal en PDF.</p>

<p>¡Gracias por elegir Transporte VIP!</p>

<p style="color: #c5a572; font-size: 12px;">
Transporte VIP - Elegancia en cada viaje<br>
Tel: +507 6123-4567 | info@transportevip.com
</p>
```

## 🎨 Personalización

### Cambiar Información del Conductor
Busca en el código HTML la sección:
```javascript
const companyInfo = {
    businessName: "TRANSPORTE VIP S.A.",
    ownerName: "Juan Carlos Rodríguez",
    // ... actualiza con tu información
};
```

### Agregar Imágenes Reales

#### Opción 1: Usar enlaces externos
1. Sube tus imágenes a [imgur.com](https://imgur.com) (gratis)
2. Copia el enlace directo
3. Reemplaza los placeholders en el HTML:
```html
<!-- Busca esta línea -->
<div class="carousel-slide active" style="background-image: url('TU_URL_AQUI');">
```

#### Opción 2: Usar imágenes locales
1. Crea una carpeta `images` en tu repositorio
2. Sube las fotos
3. Actualiza las rutas:
```html
<div class="carousel-slide active" style="background-image: url('images/ciudad-panama.jpg');">
```

### Modificar Precios de Servicios
Busca el objeto `servicePrices` en el JavaScript:
```javascript
const servicePrices = {
    cityTour: { base: 80, extraPerson: 15 },
    // ... modifica los precios aquí
};
```

## 📱 Integración con Google Maps (Opcional)

Para obtener precios más precisos basados en distancia real:

1. Obtén una API Key de Google Maps:
   - Ve a [Google Cloud Console](https://console.cloud.google.com)
   - Crea un proyecto
   - Activa "Distance Matrix API"
   - Copia tu API Key

2. Agrega el script en el HTML:
```html
<script src="https://maps.googleapis.com/maps/api/js?key=TU_API_KEY&libraries=places"></script>
```

3. Usa la API para calcular distancias reales

## 📊 Analytics (Opcional)

Para saber cuántas personas visitan tu sitio:

1. Crea una cuenta en [Google Analytics](https://analytics.google.com)
2. Obtén tu código de seguimiento
3. Agrégalo antes de `</head>`:
```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

## 🔧 Solución de Problemas

### El PDF no se genera
- Verifica que jsPDF esté cargado correctamente
- Abre la consola del navegador (F12) para ver errores

### Los emails no se envían
- Verifica tu cuenta de EmailJS
- Revisa que los IDs estén correctos
- Verifica el límite mensual (100 emails gratis)

### El sitio no se ve bien en móvil
- El diseño es responsive, pero prueba en diferentes dispositivos
- Usa Chrome DevTools para simular diferentes pantallas

## 📞 Soporte

Para agregar más funcionalidades o personalización avanzada:
- Sistema de base de datos
- Panel de administración
- Integración con pasarelas de pago
- Sistema de seguimiento GPS en tiempo real

## 🔐 Seguridad

**IMPORTANTE**: Este es un sistema básico. Para producción considera:
- Backend seguro para procesar pagos
- Base de datos encriptada
- Certificado SSL (HTTPS)
- Protección contra spam

## 📝 Licencia

Este proyecto es de uso libre. Personalízalo según tus necesidades.

## 🎯 Próximos Pasos Recomendados

1. ✅ Subir a GitHub Pages o Netlify
2. ✅ Configurar EmailJS
3. ✅ Agregar tus fotos reales
4. ✅ Actualizar información del conductor
5. ✅ Compartir el link con tus clientes
6. 🚀 Promocionar en redes sociales
7. 📈 Monitorear con Google Analytics

---

**¡Éxito con tu negocio de Transporte VIP! 🚗✨**
