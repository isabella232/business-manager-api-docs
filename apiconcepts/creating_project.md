# How to create a project using API #

In previous article, we retrieved data from TBM database. Now, let's demonstrate how create data. The approach here is the same as for retrieving data, but instead of GET method we will use POST method.
First of all, expand **Translation project** group and find an endpoint to create a project:

For example, you're going to read simple list of Units. Let's expand **Unit** group and find endpoint which allows to read list of units:

![API docs](/images/project_model.png)

In the same manner as for units, let's create a model class for translation project:

   ```cs
   public class TranslationProjectDtoEdit : BaseDtoEdit
    {
        public int? DocNum {get; set; }
        public DateTime? Date { get; set; }
        public DateTime? Term { get; set; }
        public TranslationProjectStatus? Status { get; set; }
        public string ProjectName { get; set; }
        public string Description { get; set; }
        public Guid? SourceLanguageId { get; set; }
        public Guid? CustomerId { get; set; }
        public Guid? CustomerContactId { get; set; }
        public Guid? PriceListId { get; set; }
        public string PONumber { get; set; }
        public Guid? ResponsiblePersonId { get; set; }
        public Guid? SpecializationId { get; set; }
        public string Instructions { get; set; }
        public Guid? FuzzySchemeId { get; set; }
        public Guid? QuoteId { get; set; }
        public string Folder { get; set; }
        public bool? FolderFixed { get; set; }
        public TimeSpan? EditTime { get; set; }
        public Guid? WorkflowId { get; set; }
        public Guid? AnalysisImportSettingsId { get; set; }
        public string GroupShareId { get; set; }
        public string TradosProjectId { get; set; }
        public string TradosProjectFilePath { get; set; }
        public bool? IncludeInCV { get; set; }

        public List<Guid> TargetLanguages { get; set; }
        public List<DocumentChecklistItemDtoEdit> ChecklistItems { get; set; }
    }
   ```

Now we can use this class to create a project:

   ````cs
	var client = new RestClient("https://localhost:44334/DocTranslationProject");
	var request = new RestRequest(Method.POST);
	request.AddHeader("Authorization", "Bearer your_token");

	var projectModel = new TranslationProjectDtoEdit();
	projectModel.Date = new DateTime(2021, 10, 01);
	projectModel.Term = new DateTime(2021, 10, 11);
	projectModel.ProjectName = "New project";
	projectModel.description = "Some test project";
	projectModel.sourceLanguageId = Guid.Parse("40cb29da-500a-4875-9e12-990ae5e24c14");
	projectModel.customerId = Guid.Parse("7bab4343-70c2-46ec-972f-3ddd94132271");
	projectModel.poNumber = "PO-123-3344";
	projectModel.instructions = "Some instructions";
	projectModel.TargetLanguages = new List<Guid>();
	projectModel.TargetLanguages.Add(Guid.Parse("312e2dd1-9b52-41b3-bc5e-154d4054fb85"));
	projectModel.TargetLanguages.Add(Guid.Parse("d86487b1-2eaa-4e5f-8371-684a128e84a8"));
	projectModel.TargetLanguages.Add(Guid.Parse("cd87fc65-6816-4bf6-8cf8-6610525d0560"));

	request.AddParameter("application/json", JsonConvert.SerializeObject(projectModel),  ParameterType.RequestBody);
	IRestResponse response = client.Execute(request);
	Console.WriteLine(response.Content);
   ````
Guids used in the example above can be retrieved by sending corresponding GET requests to the **Languages** and **Customers** endpoints.

If reponse status code is **201**, then you can deserialize it and retrieve ID of a created project. This ID can then be used to upload files, for example, or create jobs. 