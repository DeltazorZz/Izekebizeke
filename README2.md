Frontend - Backend

Fontos pluginok: PHP Intelephense (elefántos), Thunder Client (kérések teszteléséhez), Prettier

git clone (cmd-be) VAGY composer create-project laravel/laravel myfancyproject
(töltsd le a nodejs.org website-ról.
Ha kész, akkor futtasd a npm -v parancsot.
composer require laravel/breeze --dev
php artisan breeze:install
npm install
npm run dev)
Beállítások
composer install
.env (database)
xampp -> adatbázis létrehozása
php artisan serve
Táblák létrehozása
php artisan make:model TablaNeveNagyBetuvelEkezetNelkulEgyesSzambanPeldaulTask -m

$table->foreignId('user_id')->references('user_id')->on('users');
use HasFactory;
    protected $primaryKey = 'jelentkezo_id';
    protected $fillable = [
        'tanulo_neve',
		....
	]

php artisan make:controller TaskController -m

public function index()
    {
        $bejegyzes = Bejegyzes::all();
        return $bejegyzes;
    }
    public function show($id)
    {
        $bejegyzes = Bejegyzes::all($id);
        return $bejegyzes;
    }
    public function destroy($id)
    {
        Bejegyzes::find($id)->delete();
    }

    public function store(Request $request)
    {
        $bejegyzes = new Bejegyzes();
        $bejegyzes->tevekenyseg_id = $request->tevekenyseg_id;
        $bejegyzes->osztaly_id = $request->osztaly_id;
        $bejegyzes->allapot = $request->allapot;
        $bejegyzes->save();
    }

    public function update(Request $request, $id)
    {
        $bejegyzes = Bejegyzes::find($id);
        $bejegyzes->tevekenyseg_id = $request->tevekenyseg_id;
        $bejegyzes->osztaly_id = $request->osztaly_id;
        $bejegyzes->allapot = $request->allapot;
        $bejegyzes->save();
    }

database->migrations-> táblák:
 $table->id();
            $table->string('recept_nev');
            $table->foreignId('kat_id')->references('id')->on('kategorias');
	$table->bigInteger('votes')->default('1')
$table->date('created_at');

Task::create(['task_range_id' => 1, 'name' => 'Első HTML oldal létrehozása', 'score' => '2', 'description'=> 'Hozd létre az alap HTML oldalt']);

web.php-ban: 
Route::get('/', function () {
    return view('publicMain');
});
Route::get('/admin/felPlusSzak', [UserController::class, 'index']);

Ha paraméteres a végpont: app->Http->Middleware->VerifyCsrfToken:
	utvonal/*,
	utvonal/*/*

resources->views->csinálni kell egy alap html-t: index.blade.php
oda:  
	<script src="./js/publicMain.js" type="module"></script> 
<meta name="csrf-token" content=<?php $token = csrf_token(); echo $token; ?>>
	<link rel="stylesheet" href="./css/publicCSS.css"> 
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.1/jquery.min.js"></script>

public mappába: 
publicMain.js 
import Controller from "./js/Controller/Controller.js"
$(function(){
    new PublicController();
})

public mappa -> js és css mappa készítés
js mappába:
Model mappa: 
	AdatModel.js:
class AdatModel{
#tomb = [];
constructor(token) {
        this.#token = token;
    }

    adatBe(vegpont, myCallBack) {
        fetch(vegpont, {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
            }
        })
            .then((response) => response.json())
            .then((data) => {
                console.log('Siker:', data);
                this.#tomb = data;
                myCallBack(this.#tomb);
            })
            .catch((error) => {
                console.error('Error:', error);
            });
    }

    adatUj(vegpont, adat) {
        //console.log("elküld a modelben")
        console.log("ezt kéne megkapnia", adat)
        fetch(vegpont, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'X-CSRF-TOKEN': this.#token
            },
            body: JSON.stringify(adat),
        })
            .then((response) => response.json())
            .catch((error) => {
                console.error('Error: nem jo', error);
            });
    }

    adatModosit(vegpont, adat) {
        console.log("ADATModosit ::", adat);
        fetch(vegpont, {
            method: 'PATCH',
            headers: {
                'Content-Type': 'application/json',
                'X-CSRF-TOKEN': this.#token
            },
            body: JSON.stringify(adat),
        })
            /* .then((response) => response.json()) */
            .then(() => {
                console.log("sikeres mod");
            })
            .catch((error) => {
                console.error('Error:', error);
            });
    }

    adatTorol(vegpont, adat) {
        console.log(adat);
        console.log("Töröl: " + adat);
        fetch(vegpont, {
            method: 'DELETE',
            headers: {

                'X-CSRF-TOKEN': this.token
            },
            body: JSON.stringify(adat),
        })
            .then()
            .then(() => {
                console.log("sikeres Törlés");
            })
            .catch((error) => {
                console.error('Error:', error);
            });
    }
export default AdatModel;

	Controller mappa:
		Controller.js:
import AdatModel from "../Model/AdatModel.js";
import ReceptekView from "../View/ReceptekView.js";


class Controller{
    constructor(){
        const token = $('meta[name="csrf-token"]').attr("content");
        const adatmodel = new AdatModel(token);

        adatmodel.adatBe("/minden", this.adatok);

        $(window).on("#leves", (event)=>{
            adatmodel.adatBe("/kategoriak", event.detail);
        });
    }
    adatok(tomb){
        const szuloElem = $('main');
        new ReceptekView(tomb, szuloElem)
    }
}

export default Controller;

	View mappa:
		ReceptView.js:
class ReceptView{
    constructor(elem, szuloElem){
        szuloElem.append(`
            <tr>
                <td>${elem.recept_nev}</td>
                <td>${elem.nev}</td>
            </tr>
            
        `)
    }
}

export default ReceptView;

JelentkezesekView.js:
class JelentkezesekView{
    constructor(tomb, szuloElem){
        szuloElem.append(`
        <article>
		<table>
            <thead></thead>
            <tbody>
            <tr>
                <th>Recept neve</th>
                <th>Kategória</th>
            </tr>
                
            </tbody>
            </table>
        </article>
        `)

        this.sorElem = szuloElem.children("article:last-child");
        this.selectElem = this.sorElem.children("select")
        new JelentkezesView(tomb, this.sorElem);
        
    }
}

export default JelentkezesekView;

adatGyujtes() {
        this.opcioElem = $("#szak").find(":selected").val();
        this.#jelentkezoAdat.inditott_id = $("#szak").val();
        this.#jelentkezoAdat.tanulo_neve = $("#tanulo_neve").val();
        this.#jelentkezoAdat.email = $("#email").val();
        this.#jelentkezoAdat.telefonszam = $("#telefonszam").val();
    }

OpcioView.js
class OpcioView{
    #elem;
    constructor(elem, szuloElem){
        this.#elem = elem;
        var szak_ertek = elem.inditott_id
        szuloElem.append(`
        <option id="opcio" value="${szak_ertek}">${elem.megnevezes}</option>
        `)
        this.opcioElem = szuloElem.children("option");
    }
}

export default OpcioView;

Raw sql lekérdezés:
laravel 10-nél!! You don't need to use DB::raw() inside DB::select():
public function szak_indittotSzak(){
        $szakok = DB::select"select * from szaks sz, inditott_szaks isz where sz.szak_id = isz.szak_id ");
        return $szakok;
    }



Form elem+ select elem + adatgyűjtés:
<h1 class="cim">Jelentkezési oldal</h1>
        <form id="jelentkezes" name="jelentkezes" >
        <label for="tanulo_neve" class="form-label">Név:</label>
        <input type="text" id="tanulo_neve" name="tanulo_neve" class="form-control" placeholder="Minta Pista" requried>
        <label for="szak" class="form-label">Szak kiválasztása:</label>
        <select name="inditott_id" id="szak" class="form-select" >
        </select>
        <input type="checkbox" id="adatkez" name="adatkez" >
        <label for="adatkez">Elfogadom az <a href="#" target="_blank">adatkezelési szabályzatot</a></label><br>
        <input type="button" id="elkuld" value="Elküld" class="btn btn-outline-secondary" disabled>
        </form>
        `);

        this.formElem = szuloElem.children("form:last-child");
        this.selectElem = this.formElem.children("select");
        tomb.forEach((opcio) => {
            const opciom = new OpcioView(opcio, this.selectElem);
 });
 this.elkuldElem = $(`#elkuld`);
        this.elkuldElem.on("click", () => {
            this.adatGyujtes();
            this.kattintastrigger("elkuld");
            
        });
adatGyujtes() {
        this.opcioElem = $("#szak").find(":selected").val();
        this.#jelentkezoAdat.inditott_id = $("#szak").val();
        this.#jelentkezoAdat.tanulo_neve = $("#tanulo_neve").val();
        this.#jelentkezoAdat.email = $("#email").val();
        this.#jelentkezoAdat.telefonszam = $("#telefonszam").val();
    }

    kattintastrigger(esemenyNeve) {
        const esemeny = new CustomEvent(esemenyNeve, {
            detail: this.#jelentkezoAdat,
        });
        window.dispatchEvent(esemeny);
    }

