# REST-API-User
/////////////////Config Code With mongoosedb
const mongoose=require('mongoose');
mongoose.connect('mongodb://127.0.0.1:27017/webProject').then((req,res)=>{

console.log("conected..");
}).catch((e)=>{

    console.log(e);
})
///////////////////////////User Entitiy Code.
const mongoose=require('mongoose');
const mongo=mongoose.Schema({

    name:String,
    caste:String,
    address:String

});
module.exports=mongoose.model('Employee',mongo);


//////////////////////////API Code.
const express = require('express');
require('./config');
const user = require('./User');
const app = express();

app.use(express.json());


app.post('/user', async (req, res) => {
    let data = new user(req.body);
    let result = await data.save();
    res.send(result);

});

app.put('/update/:_id', async (req, res) => {
    let data = await user.updateOne(req.params,
        { $set: req.body }
    );
    res.send(data);

});

app.get('/',async(req,res)=>{
    let data=await user.find();
    console.log(data);
    res.send(data);

})

app.delete('/delete/:_id',async(req,res)=>{
    let data=await user.deleteOne(req.params);
    res.send(data);
})

app.get('/user/:_id',async(req,res)=>{

    try {
        let data=await user.find(req.params);
     if (data) {
          res.status(200).json(data);
        } else {
          res.status(404).json({ error: 'User not found' }); 
        }
      } catch (error) {
        res.status(500).json({ error: 'Server error' });       }
    })

app.listen(3100);
