# Filtrar Datos MVC
http://cristiantorresalfaro.blogspot.com/2019/07/filtrar-datos-mvc.html

<div style="text-align: justify;">
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;"><span style="background-color: white; font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;">En esta ocasión&nbsp;veremos la manera de como filtrar datos(DropDowlist) en una aplicación&nbsp;mvc de asp.net y como recuperar un valor por medio de ajax dependiendo del valor seleccionado.</span></span></div>
<br />
<div style="text-align: justify;">
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;"><br /></span></div>
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;"></span><br />
<div style="text-align: justify;">
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;"><span style="background-color: white;">Imaginemos el siguiente escenario&nbsp;&nbsp;tenemos 2 tablas Cliente y&nbsp;</span>Crédito en donde un cliente puede tener varios créditos, lo que aremos es crear una vista en donde mostraremos los datos del cliente y al seleccionar un cliente mostraremos el detalle de todos los créditos&nbsp;que el cliente posee.</span></div>
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;">
</span>
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;"><br /></span>
<br />
<div class="separator" style="clear: both; text-align: center;">
<a href="https://drive.google.com/uc?id=1Kl65wexcLpngF4pdO2otHAwdUYsJWBj-" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="208" data-original-width="372" src="https://drive.google.com/uc?id=1Kl65wexcLpngF4pdO2otHAwdUYsJWBj-" /></a></div>
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;"><br /></span>
<br />
<div style="text-align: justify;">
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;"><span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;">Empezaremos creando la base de datos, la cual tendrá&nbsp;la siguiente estructura.</span></span></div>
<br />
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;"><br /></span>
<br />
<div class="separator" style="clear: both; text-align: center;">
<a href="https://drive.google.com/uc?id=175UbhozscO4o_t4BNBljv4axuXNhzPGH" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="199" data-original-width="612" height="130" src="https://drive.google.com/uc?id=175UbhozscO4o_t4BNBljv4axuXNhzPGH" width="400" /></a></div>
<div style="text-align: justify;">
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;">Creamos un nuevo proyecto MVC y generamos nuestro modelo</span></div>
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;"><br /></span>
<br />
<div class="separator" style="clear: both; text-align: center;">
<a href="https://drive.google.com/uc?id=1fNzjhXhS8eKAGZngRTa3AIKyWyNxN6HN" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="292" data-original-width="558" height="208" src="https://drive.google.com/uc?id=1fNzjhXhS8eKAGZngRTa3AIKyWyNxN6HN" width="400" /></a></div>
<br />
<div style="text-align: justify;">
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;">Ahora crearemos una clase model que nos servirá&nbsp;para mostrar los datos(creamos una carpeta ViewModel dentro de Model y creamos la clase en mi caso le llamare&nbsp;<b>ClienteModel</b>)</span></div>
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;"><br /></span>
<br />
<div class="separator" style="clear: both; text-align: center;">
<a href="https://drive.google.com/uc?id=1DVFShxy00oMZDjUgfKT3IhnoUqYeOQIG" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="373" data-original-width="351" height="320" src="https://drive.google.com/uc?id=1DVFShxy00oMZDjUgfKT3IhnoUqYeOQIG" width="301" /></a></div>
<div class="separator" style="clear: both; text-align: center;">
<br /></div>
<div class="separator" style="clear: both; text-align: center;">
<br /></div>
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;"><br /></span>
<br />
<pre class="brush: csharp"> 
    public class ClienteModel
    {
        public int Id { get; set; }
        public string Nombre { get; set; }
        public string Dui { get; set; }
        public decimal Saldo { get; set; }
        public string Credito { get; set; }
    
</pre>
<br />
<div style="text-align: justify;">
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;">Esta clase lo único que tiene son las propiedades que usaremos para mostrar los datos en nuestras vistas.
En nuestro controlador para listar los clientes aremos lo siguiente en nuestro método Index.
</span></div>
<pre class="brush: csharp"> 
        public ActionResult Index()
        {
            List<clientemodel> clientes;
            using(CreditosEntities bd = new CreditosEntities())
            {
                //obtengo listado clientes
                clientes = (from cli in bd.Cliente
                            select new ClienteModel
                            {
                                Id = cli.Id_Cliente,
                                Nombre = cli.Nombre,
                                Dui = cli.Dui
                            }).ToList();
            }

            return View(clientes);
        }
</pre>
<br />
<div style="text-align: justify;">
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;">Lo que hacemos es retornar un listado con los datos de los clientes haciendo uso de nuestra clase modelo.
Ahora agreguemos nuestra vista Index la cual tendrá el siguiente código.
</span></div>
<pre class="brush: xml"> 
&lt;div class="row"&gt;
    &lt;div class="col-md-12"&gt;

        &lt;table class="table"&gt;
            &lt;thead class="thead-dark"&gt;
                &lt;tr&gt;
                    &lt;th&gt;ID&lt;/th&gt;
                    &lt;th&gt;DUI&lt;/th&gt;
                    &lt;th&gt;NOMBRE&lt;/th&gt;
                    &lt;th&gt;Creditos&lt;/th&gt;
                &lt;/tr&gt;
            &lt;/thead&gt;
            <!-- Recorro el modelo para imprimir datos -->
            @foreach (var elemento in Model)
            {
                &lt;tr&gt;
                    &lt;td&gt;@elemento.Id&lt;/td&gt;
                    &lt;td&gt;@elemento.Dui&lt;/td&gt;
                    &lt;td&gt;@elemento.Nombre&lt;/td&gt;
                    &lt;td&gt;
                        <!-- Boton-->
                        &lt;input type="button" class="btn btn-danger" value="Ver Creditos"
                               onclick="document.location.href='@Url.Content("~/Cliente/ConsultarCreditos/"+elemento.Id)'" /&gt;

                    &lt;/td&gt;
                &lt;/tr&gt;
            }



        &lt;/table&gt;
    &lt;/div&gt;
&lt;/div&gt;
</pre>
<br />
<div class="separator" style="clear: both; text-align: center;">
<a href="https://drive.google.com/uc?id=13LbF1RYL-UJK7M6AtsEmL8uPRs-wHx4n" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="216" data-original-width="789" height="87" src="https://drive.google.com/uc?id=13LbF1RYL-UJK7M6AtsEmL8uPRs-wHx4n" width="320" /></a></div>
<br />
<div style="text-align: justify;">
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;">Ahora crearemos el método para obtener la información de los créditos del cliente seleccionado en nuestro controlador agregamos el siguiente código.
</span></div>
<pre class="brush: csharp"> 
        //variable global
        CreditosEntities bd = new CreditosEntities();

        public ActionResult ConsultarCreditos(int id)
        {

            //obtengo id del cliente
            ClienteModel clien = new ClienteModel();
            {
                clien.Id = id;
            };

            // cargar drow creditos de cliente seleccionado mediante id
            ViewBag.Credito= new SelectList(bd.Credito.Where(c =&gt; c.Id_Cliente == id).ToList(), "N_Credito", "N_Credito");

            //obtener nombre
            ViewBag.Nombre = bd.Cliente.Where(c =&gt;c.Id_Cliente ==id).First().Nombre;
            return View(clien);
        }

        //Metodo para obtener saldo por ajax jquery
        public JsonResult Saldo(string credito)
        {
            return Json(bd.Credito.First(c =&gt; c.N_Credito == credito).Saldo, JsonRequestBehavior.AllowGet);
        }
</pre>
<br />
<div style="text-align: justify;">
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;">Tenemos 2 métodos en el primero <b>ConsutarCreditos</b> es donde por medio del id del cliente seleccionado obtendremos todos los créditos del cliente (esto lo hacemos en viewbag para mostrarlo en nuestra vista en un dropdowlist) también recuperamos el nombre del cliente seleccionado.</span></div>
<div style="text-align: justify;">
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;">El método <b>Saldo</b> nos servirá para obtener el saldo cuando seleccionemos el numero de crédito en el dropdowlist(retorna un json para hacerlo fácil con ajax).

Ahora creamos nuestra vista ConsultarCreditos.
</span></div>
<pre class="brush: xml"> 
@model FiltrarDatosMVC.Controllers.ViewModels.ClienteModel
@{
    ViewBag.Title = "ConsultarCreditos";
}

&lt;h2&gt;Consultar Creditos&lt;/h2&gt;

@using (Html.BeginForm("ConsultarCreditos", "Cliente", FormMethod.Post))
{
    &lt;div class="row"&gt;
        &lt;div class="col-md-12"&gt;
            &lt;div class="form-group"&gt;
                &lt;label&gt;Cliente:&lt;/label&gt;@Html.TextBox("txtNombre", (string)ViewBag.Nombre, new { disabled = true })&lt;br /&gt;
            &lt;/div&gt;
            &lt;div class="form-group"&gt;
                &lt;label&gt;Cuentas del Cliente:&lt;/label&gt;  @Html.DropDownListFor(m =&gt; m.Credito, (SelectList)ViewBag.Credito, "seleccione")
                &lt;br /&gt;
            &lt;/div&gt;
            &lt;div class="form-group"&gt;
                &lt;label&gt;Saldo:&lt;/label&gt;@Html.TextBoxFor(m =&gt; m.Saldo, null, new { disabled = true })&lt;br /&gt;
            &lt;/div&gt;

            &lt;input type="button" class="btn btn-primary" value="Salir" onclick="document.location.href='@Url.Content("~/Cliente/Index")'" /&gt;
        &lt;/div&gt;
    &lt;/div&gt;

}

@Scripts.Render("~/bundles/jquery")

&lt;script&gt;
      //muestro saldo depende de credito seleccionado
    $("#Credito").change(function () {
        $.ajax({
            url: '@Url.Action("Saldo","Cliente")',
            data: { credito: $(this).val() },
            success: function (a) {
                $("#Saldo").val(a);
            }
        });
    });

&lt;/script&gt;
</pre>
<br />
<div style="text-align: justify;">
<span style="font-family: &quot;arial&quot; , &quot;helvetica&quot; , sans-serif;">Lo que hacemos acá es primeramente mostrar los datos del cliente nombre y créditos que posee(mostramos los créditos en un dropdowlist) y cuando seleccionamos un crédito nos muestra el saldo de este para eso hacemos uso de ajax con jquery(usamos el método Saldo de nuestro controlador)
</span></div>
<div class="separator" style="clear: both; text-align: center;">
<a href="https://drive.google.com/uc?id=1Kl65wexcLpngF4pdO2otHAwdUYsJWBj-" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="208" data-original-width="372" height="223" src="https://drive.google.com/uc?id=1Kl65wexcLpngF4pdO2otHAwdUYsJWBj-" width="400" /></a></div>
