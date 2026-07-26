# MERCER · Gestión de Stock de Alfombras

App web single-file (React + Tailwind por CDN) con sincronización en la nube vía Supabase.
Diseño con la estética Mercer (neutros cálidos, tipografía serif).

## Contenido del ZIP
- `index.html` — la app completa (149 alfombras reales ya cargadas como datos de prueba)
- `vercel.json` — config de deploy
- `supabase.sql` — script para crear la base de datos

---

## 1) Usuarios y claves
| Usuario         | Clave      | Rol                                   |
|-----------------|------------|---------------------------------------|
| Administrador1  | Mercer1    | Acceso completo (alta/edición/borrado/exportar) |
| Administrador2  | Mercer2    | Acceso completo                        |
| Administrador3  | Mercer3    | Acceso completo                        |
| Consulta1       | Consulta1  | Solo lectura y búsqueda                |

---

## 2) Probar YA (modo local)
Abrí `index.html` en cualquier navegador. Funciona con los 149 productos de prueba
guardados en el navegador (localStorage). **Este modo NO sincroniza entre dispositivos.**

---

## 3) Activar sincronización en la nube (recomendado)
Para que todos vean los mismos cambios en tiempo real desde cualquier PC/celular:

1. Entrá a tu proyecto Supabase → **SQL Editor** → **New query**.
2. Pegá todo el contenido de `supabase.sql` y hacé **Run**.
3. En Supabase → **Settings → API**, copiá:
   - **Project URL** (ej: `https://xxxx.supabase.co`)
   - **anon public** key
4. Abrí `index.html`, buscá arriba estas dos líneas y completalas:
   ```js
   const SUPABASE_URL  = "";  // ← pegá tu Project URL
   const SUPABASE_KEY  = "";  // ← pegá tu anon public key
   ```
5. Guardá. La primera vez que ingreses, la app sube automáticamente las 149 alfombras
   a Supabase. Desde ahí, todos los cambios se guardan en la nube y se refrescan solos
   (cada 8 segundos) en todos los dispositivos conectados.

---

## 4) Publicar en Vercel
- **Opción rápida:** arrastrá los 3 archivos a Vercel (Add New → Project → Deploy).
- **Vía GitHub:** subí los archivos al repo → conectá el repo en Vercel → deploy automático.
  Para editar: descargás el ZIP → editás → pegás en el editor de GitHub → commit → Vercel redeploya.

> Importante: los archivos deben quedar en la **raíz** del proyecto (no dentro de una carpeta).

---

## Funcionalidades
- **Dashboard:** totales, valor de inventario, contenedores, últimos ingresos, gráficos de stock por contenedor y compras por mes.
- **Inventario / Compras:** buscar por nombre/código, filtrar por contenedor/mes/año, ordenar columnas, paginación configurable, +/- de stock inline. Los productos **sin stock (0) se ocultan** automáticamente; se pueden ver con el botón "Ver agotados". La columna de precio de compra se muestra como **Costo**.
- **Reportes:** valor total, stock por contenedor, por mes y top productos con gráficos.
- **Ventas:** registrar venta con fecha, producto, modelo, medida, cantidad, **estado** (Cobrada y entregada / Cobrada y falta entregar / Entregada y falta cobrar), **condición de venta** (Minorista, Minorista 10/20/30%, Mayorista), **facturación** (Factura A / Sin Factura), **canje** (entregas a influencers sin cobro), precio unitario, **costo**, importe total con descuento automático, **margen** (venta - costo, en $ y %), cliente, **teléfono**, vendedora y observaciones. Al guardar, **descuenta el stock automáticamente**. Lista de últimas 10 ventas con búsqueda, filtro por estado, edición y anulación (solo admin).
- **Dashboard de Ventas:** ventas del día/mes, productos vendidos, facturación total, ticket promedio, ranking de productos, ventas y participación por vendedora, evolución diaria.
- **Alta / Edición / Eliminación** (con confirmación) para administradores.
- **Exportar** a Excel, CSV y PDF. **Importar** desde Excel (actualiza por código).
- **Sincronización en la nube (Supabase):** stock y ventas se guardan de forma permanente y se refrescan solos cada 8 segundos en todos los dispositivos conectados.
- **Login** con 3 admins + 1 consulta. Responsive (PC, tablet, celular).

### Nota sobre las columnas nuevas
El archivo `supabase.sql` crea las tablas `alfombras` (stock) y `ventas`, y agrega las columnas
nuevas de ventas (telefono, estado, condicion, facturacion, costo_unitario, canje).
**Si ya habías corrido el SQL antes, volvé a ejecutarlo:** sólo agrega lo que falta, no borra datos.
