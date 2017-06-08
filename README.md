# guia_rapido_mongodb

Instalação: https://docs.mongodb.org/manual/tutorial/install-mongodb-on-windows/

Teste:

$ mongo --port 27017 --host localhost
MongoDB shell version: 2.6.6
connecting to: test
Acessar mongo shell: $ mongo

Exibir bancos: > show dbs

Usar banco: > use seubanco

Ver banco atual: > db

Inserir objeto no em coleção:

> var contato = { "nome" : "Nome do Contato" }
> db.contatos.insert(contato)
WriteResult({ "nInserted" : 1 })
Ver coleções do banco atual: > show collections

Buscar documentos de uma coleção: > db.contatos.find()

Excluir banco:

> use seubanco
> db.dropDatabase()
Buscar um registro: > db.contatos.findOne()

Buscar um contato por e-mail:

> var criterio = { "email" : "cont2@empresa.com.br" }
> var contato = db.contatos.find(criterio)
> contato
{
    "_id" : ObjectId("530b8260b61fac75624ccfd2"),
    "nome" : "Contato 2 Mongo",
    "email" : "cont2@empresa.com.br"
}
Buscar um contato com expressão regular:

> var criterio = { "nome" : /tato/i }
> var contatos = db.contatos.find(criterio)
Count de registros: > db.contatos.count()

Count com filtro: > db.contatos.count({ "nome" : /to 2/i })

Consulta com OR:

> db.contatos.find({
    "$or" : [
        {
            "email" : "cont2@empresa.com.br"
        },
        {
            "nome" : "Contato 1 Mongo"
        }
])
Consulta negando filtro:

> db.contatos.find({
    "email" : {
        "$ne" : "cont2@empresa.com.br"
    }
});
Criando índice: > db.contatos.ensureIndex({ "email": 1 }) 1 para ascendente, -1 para descendente

Buscar índices: > db.contatos.getIndexes()

Apagar índice: db.contatos.dropIndex('email_1')

Criar índice único: db.contatos.ensureIndex( { email: 1 }, { unique: true } )

Consulta com projeção (retorna só a coluna 'nome' e '_id'): > db.contatos.find({}, { "nome": 1 })

Consulta com projeção (retorna só a coluna 'nome', sem '_id'): > db.contatos.find({}, { "nome": 1, _id: 0 })

Removendo um documento usando filtro: > db.contatos.remove({ "email": "email@email.com.br" })

Apagar todos os dados: > db.contatos.remove()

Atualizar registro: > db.contatos.update({/*criterio*/}, {/*objeto contato*/})

Atualizar, se não existir, insere: > db.contatos.update({/*criterio*/}, {/*objeto contato*/}, true)

Atualizar apenas um atributo do documento:

> db.contatos.update(
    { "email" : "cont4@empresa.com.br"},
    {
        "$set" :
            {
                "nome" : "Mais uma alteração"
            }
    }
)
WriteResult({ "nMatched" : 1, "nUpserted" : 0 })
Transactions: https://docs.mongodb.org/manual/tutorial/perform-two-phase-commits/

Criando um atributo de relacionamento:

> contato.emergencia = {
    "$ref" : "contatos", /* coleção da referencia */
    "$id" : emergencia._id /* ObjectID da referencia */
};

// resolvendo uma referencia
> function refResolver(ref) {
    return db[ref.$ref].findOne({"_id": ref.$id})
}
