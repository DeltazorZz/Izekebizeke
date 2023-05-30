# Izekebizeke
Java
Szerializáció + Deszerializáció:
implementálni!!
private void szerializalas() {
        FileOutputStream kiFajl;
        try {
            kiFajl = new FileOutputStream("adat.ser");
            ObjectOutputStream kiObj = new ObjectOutputStream(kiFajl);
            kiObj.writeObject(kartyak);
            kiObj.close();
        } catch (FileNotFoundException ex) {
            Logger.getLogger(Szerializacio.class.getName()).log(Level.SEVERE, null, ex);
        } catch (IOException ex) {
            Logger.getLogger(Szerializacio.class.getName()).log(Level.SEVERE, null, ex);
        }
    }

private void deszerializalas(){
        FileInputStream beFajl;
        try {
            beFajl = new FileInputStream("adat.ser");
            ObjectInputStream beObj = new ObjectInputStream(beFajl);
            kartyak = (ArrayList<Kartya>)beObj.readObject();
            beObj.close();
        } catch (FileNotFoundException ex) {
            Logger.getLogger(Szerializacio.class.getName()).log(Level.SEVERE, null, ex);
        } catch (IOException | ClassNotFoundException ex) {
            Logger.getLogger(Szerializacio.class.getName()).log(Level.SEVERE, null, ex);
        }
    }
private void tartalom(String cim) {
        System.out.println(cim);
        if (kartyak != null) {
            for (Kartya kartya : kartyak) {
                System.out.println(kartya);
            }
        } else {
            System.out.println("üres a lista");
        }
    }

Rendezés:
	Collections.sort(lista);

Két pl Ember típus nevének összehasonlítása:
public class Ember implements Comparable<Ember>
implementálás után villanykörte-> Implements all abstract ..

@Override
    public int compareTo(Ember o) {
        Collator coll = Collator.getInstance();
        return coll.compare(this.nev, o.nev);
    }

Fájlbaírás:
//Files (egy sorba képes írni)
private void fajlbaIras1() {  
String kimenet = "Ez egy sor.";
  
        try {
            Files.write(Paths.get("adat1.txt"), kimenet.getBytes());
        } catch (IOException ex) {
            Logger.getLogger(Mentes.class.getName()).log(Level.SEVERE, null, ex);
        }
    }

//Hash map + BufferedWriter (megtudja törni)
private static void fel3() {
        
        Map<String, Integer> kiesbe = new HashMap<>();
        for (Adatok adat : adatok) {
            String kulcs= "";
            if (adat.getMerre().equals("be") || adat.getMerre().equals("ki")) {
               kulcs = adat.getAzonosito();
            }else{
                kulcs = "semmi";
            }
            if (kiesbe.containsKey(kulcs)) {
                int ertek = kiesbe.get(kulcs);
                kiesbe.replace(kulcs, ertek+1);
            }else{
            kiesbe.put(kulcs, 1);
            }
        }
     
        try {
            BufferedWriter bf = new BufferedWriter(new FileWriter("athaladas.txt"));
            for (Map.Entry<String, Integer> entry : kiesbe.entrySet()) {
                bf.write(entry.getKey()+":"+String.valueOf(entry.getValue()));
                bf.newLine(); //megtudja törni
            }
            bf.close();
            
        } catch (IOException ex) {
            Logger.getLogger(Tarsalgo.class.getName()).log(Level.SEVERE, null, ex);
        }
        } 
    }

FájlbólOlvas:
//Files
private void fajlbolOlvas1() {
        try {
            List<String> sorok = Files.readAllLines(Paths.get("adat1.txt"));
            System.out.println("adat1 sorai:");
            for (String sor : sorok) {
                System.out.println(sor);
            }
        } catch (IOException ex) {
            Logger.getLogger(Mentes.class.getName()).log(Level.SEVERE, null, ex);
        }
    }

//BufferedW
private void fajlbolOlvas2() {
        try {
            BufferedReader br = new BufferedReader(new FileReader("adat2.txt"));
            String sor;
            while((sor = br.readLine()) != null){
                System.out.println(sor);
            }
            br.close();
        } catch (FileNotFoundException ex) {
            Logger.getLogger(Mentes.class.getName()).log(Level.SEVERE, null, ex);
        } catch (IOException ex) {
            Logger.getLogger(Mentes.class.getName()).log(Level.SEVERE, null, ex);
        }
    }

Listből remove + nem jó a foreach
for (int i = 0; i < kertem.size(); i++) {
            if (kertem.get(i).getAzon().equals(viragAzon)) {
                kertem.remove(kertem.get(i));
            }
        }

Hashmap példák:
private static void fel5() {
        Map<String, Integer> stat = new HashMap<>();
         for (Adatok adat : adatok) {
         String kulcs = "";
             if (adat.getOperator().equals("+")||adat.getOperator().equals("-")||adat.getOperator().equals("*")||adat.getOperator().equals("/")||adat.getOperator().equals("div")||adat.getOperator().equals("mod")) {
                 kulcs = adat.getOperator();
             }else{
             kulcs = "kecske";
             }
             
             if (stat.containsKey(kulcs)) {
                 int ertek = stat.get(kulcs);
                 stat.replace(kulcs, ertek+1);
             }else{
             stat.put(kulcs, 1);
             }            
         }
         for (Map.Entry<String, Integer> entry : stat.entrySet()) {
            String key = entry.getKey();
            Integer value = entry.getValue();
             if (!key.equals("kecske")) {
                 System.out.printf("%s: %d\n", key, value);
             }
        }
    }

Csak megszámolja hány olyan nevű növény van
Map<String, Integer> stat = new HashMap<>();
        for (Noveny noveny : kertem) {
            String kulcs = noveny.getNev();
            
            if (stat.containsKey(kulcs)) {
                int ertek = stat.get(kulcs);
                stat.replace(kulcs, ertek+1);
            }else{
            stat.put(kulcs, 1);
            }
        }
         for (Map.Entry<String, Integer> entry : stat.entrySet()) {
            String key = entry.getKey();
            Integer value = entry.getValue();
                System.out.printf("%s: %d\n", key, value);
        }

GUI
MVC modell!:
	program mappa:
		modell mappa: Kartya.java
		nezet mappa: KartyaEpito.form
	kepek mappa

Legyen ini(), amit meghívok a class-ban
	
CsomagMentése:
JFileChooser dlgMent = new JFileChooser();
        dlgMent.showSaveDialog(rootPane);
        fajlMentese();

private void fajlMentese(String fp) {
        String kimenet = "";
        for (Kartya kartya : kartyak) {
            kimenet += kartya.getKerdes();
            kimenet += "/";
            kimenet += kartya.getValasz();
            kimenet += "\n";
        }
        try {
            Files.write(Paths.get(fp), kimenet.getBytes());
        } catch (IOException ex) {
            Logger.getLogger(KartyaEpito.class.getName()).log(Level.SEVERE, null, ex);
        }
 	   }

CsomagVálaszt:
JFileChooser dlgNyit = new JFileChooser();
        dlgNyit.showOpenDialog(rootPane);
        fajlMegnyitasa();

Szöveg beállítása:
txtKerdes.setText("");

Kartya obj készítése:
private void kartyatKeszit() {
        String kerdes = txtKerdes.getText();
        String valasz = txaValasz.getText();
        Kartya k = new Kartya(kerdes, valasz);
        kartyak.add(k);
    }

JOP + kilépés

private void kilepes() {
        int valasz = JOptionPane.showConfirmDialog(rootPane, "Biztos kilépsz?", "Kilépés", JOptionPane.YES_NO_OPTION);
        System.out.println("valasz = " + valasz);
       
        if(valasz == JOptionPane.YES_OPTION){
            System.exit(0);
        }
    }

Eredeti kilép inaktiv
setDefaultCloseOperation(javax.swing.WindowConstants.DO_NOTHING_ON_CLOSE);

Kép használata JMenuItem:
JMenuItem(String text, Icon icon) vagy JMenuItem(Icon icon)
JMenuItem mnuPrgUjra = new JMenuItem("Újra", new ImageIcon(this.getClass().getResource("/kepek/folder.gif")));

List tartalma:
/* elemek lekérdezése */
        ListModel<String> model = jList1.getModel();
        String s = "";
        for (int i = 0; i < model.getSize(); i++) {
            s += model.getElementAt(i);
        }

/* új elemek megadása */
        DefaultListModel<String> dlm = new DefaultListModel<>();
        dlm.addElement("első");
        dlm.addElement("uccsó");
        dlm.add(1, "középső");
        jList1.setModel(dlm);

/* jList1 tartalma */
        DefaultListModel<String> dlm = new DefaultListModel<>();
        ListModel<String> lm = jList1.getModel();
        for (int i = 0; i < lm.getSize(); i++) {
            dlm.addElement(lm.getElementAt(i));
        }

Hozzáfűzés:
        ComboBoxModel<String> cbm = cmbSzak.getModel();
        boolean elejere = true;
        for (int i = 0; i < cbm.getSize(); i++) {
            String elem = cbm.getElementAt(i);
            if(elejere){
                dlm.add(i, elem);
            }else{
                dlm.addElement(elem);
            }
        }
       
        jList1.setModel(dlm);

Szerkeszthető e valami:
void rdbKapcsol(boolean szerkesztheto){
        jRadioButton6.setEnabled(szerkesztheto);
        jRadioButton7.setEnabled(szerkesztheto);
    }

Checkbox kiválasztás
private void jCheckBox1ItemStateChanged(java.awt.event.ItemEvent evt) {
        if(jCheckBox1.isSelected()){
            rdbBe();
        }else{
            rdbKi();
        }
    }
 private void rdbBe(){
        jRadioButton6.setEnabled(true);
        jRadioButton7.setEnabled(true);
    }
   
    private void rdbKi(){
        jRadioButton6.setEnabled(false);
        jRadioButton7.setEnabled(false);
       
    }


Új elem combóba:
	combo.addItem(“új elem”);
