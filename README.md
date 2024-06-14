const mongoose=require('mongoose');//mongooseni obe'ktga olish
mongoose.connect('mongodb://localhost/invertory')//mongoosening connect metodi bizga promise qaytaradi
.then(()=>{//va uning then() va catch() methodlari bor
    console.log('MongoDBga ulanish hosil qilindi...')
})
.catch((err)=>{//catch()
    console.error('MongoDBga ulanish paytida xatolik yuz berdi',err);
})
const sizeSchema=new mongoose.Schema({
    h:Number,
    w:Number,
    uom:String
});
const invertorySchema=new mongoose.Schema({
    item:String,
    qty:Number,
    size:sizeSchema,
    status:String
});
const Invertory= mongoose.model('Invertory',invertorySchema);
async function createdInvertory(){
const invertory=new Invertory({
item:"planner",
qty:50,
size:{
    h:25,
    w:35,
    uom:"cm"
},
status:"B"
});
const savedInvertory=await invertory.save();
console.log(savedInvertory);
}
createdInvertory();
async function getInvertory1(){
    return await Invertory
    .find({status:'A'})
    .sort({item:1})
    .select({item:1,qty:1,_id:0});
}
async function getInvertory2(){
    return await Invertory
    .find()
    .or([{qty:{$lte:50}},{item:/.*p.*/i}])
    .sort({qty:-1});
}

async function getElement(){
    return await Invertory
    .find()
    .or([{qty:{$lte:30}},{item:'A'}])
    .sort({item:-1})
    .select({item:-1,qty:-1});
}
async function hiInventory(){
    return await Invertory
    .find({item:/.*p.*/i})
    .sort({qty:1});
}
async function run(){
    const item= await hiInventory();
console.log(item);
}
run();
async function updateInventory1(id){
    const invertory=await Invertory.findById(id);
    if(!invertory)
        return;
    
    invertory.item='daftar'
const updatedInventory=await invertory.save();
console.log(updatedInventory);
}
async function updateInventory2(id){
 const result=await Invertory.updateOne({_id:id},{
    $set:{
        item:'Banner',
        qty:60
    }
 });
console.log(result);
}
updateInventory2('665de955c8737e740eb87f33');
//mongooseda Schema
const codersSchema = new mongoose.Schema({
    name:String,
    job:[String],//Schema yordamida mongoddb dagi qiymatlarni,
    salary:String,//qaysi turga tegishli ekanligini belgilab olamiz.
    project:[String],
    tags:[String],
    company:String,
    isWorkNow:Boolean
});
//mongoosedagi model tushunchasi
const Coder=mongoose.model('coder',codersSchema);

async function createDevelopers(){//mongodb ga saqlash
    const coder=new Coder({ //Coder() bizda class hisoblanadi shuning u-n kattada yozilgan
    name:'Izzatbek Abdusharipov',
    job:['softwere engineer','nodejs backend team leader','frontend team leader','project menegment'],
    salary:'350000$',
    project:['readBRG','al-Kharezm universion group','algoPower','nesassary product'],
    tags:['coder','dasturlash'],
    company:'Google',
    isWorkNow:true
});
const savedCoder = await coder.save();//mongodb saqlash u-n save methodidan foydalanamiz
console.log(savedCoder);
}
//mongodb dan hujjatlarni o'qib olish
async function getCoders(){
    const ourcoder= await Coder.find({
        salary:'350000$'
    })
    .limit(2);
    console.log(ourcoder);
}

async function getCoders(){//faqat bitta tanlab olish bazadan
    const ourcoder=await Coder.findOne({//
        company:'Google'
    })
    .select({name:'Izzatbek Abdusharipov'});
    
console.log(ourcoder);
}
async function getCoders(){
    const ourcoder=await Coder.find()
    // .and({name:'Izzatbek Abdusharipov'},{isWorkNow:true})
    // .countDocuments();
    .or({name:'Izzatbek Abdusharipov'},{name:'Ismoil Sapayev'})
    .countDocuments();
console.log(ourcoder);
}
async function getCoders(){
    const pageNumber=3;
    const pageSize=10;
const ourcoder=await Coder.find({name:/il/i})
.skip((pageNumber-1)*pageSize)//pagination qilish
.limit(pageSize)//yani saxifalarga bolish
console.log(ourcoder);
}

async function getCoders(){
const ourcoder=await Coder.find({name:/.*Izz*./})
.select({tags:1});
console.log(ourcoder);
}
getCoders();//va Funktsiyani chaqirib qo'yamiz
mongoose.connect('mongodb://localhost/test')
.then(()=>{
    console.log('MongoDBga ulanish hosil qilindi...')
})
.catch((err)=>{
    console.log('MongoDBga ulanish nosozlik yuz berdi!')
});
const bookSchema=new mongoose.Schema({
    name:String,
    author:String,
    tags:[String],
    isPublished:Boolean
});

const Book=mongoose.model('book',bookSchema);
async function createBook(){
    const book=new Book({
        name:'anvar narzullayev',
        author:'mohirdev',
        tags:['mohirdev','sariqdev','python darslari'],
        isPublished:true
    });
    const savedBook= await book.save();
console.log(savedBook);
}
//createBook();
async function getSomeBook(){
    return await Book
    .find()
    .or([{name:'anvar narzullayev'},{author:'mohirdev'}])
    .sort({name:1})
    .select({name:1,tags:1,_id:0});
}
async function running(){
    const getElement=getSomeBook();
console.log(getElement);
}
running();
