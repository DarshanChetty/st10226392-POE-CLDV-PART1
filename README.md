# st10226392-POE-CLDV-PART1

using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;

namespace POEPart1
{
  
        public static class Function1
        {
            [FunctionName("St10226392")]
            public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = "id")] HttpRequest req,
        ILogger log)
            {
                log.LogInformation("C# HTTP trigger function processed a request.");

                string[] ids = { "0000000000000", "7272727272727", "3986533009955" };
                string[] names = { "Ed", "Jake", "Phil" };
                string[] centres = { "Dischem", "Alpha Pharmacy", "Momentum" };
                string[] vaccines = { "Pfizer", "J&J", "Pfizer" };
                string[] vaccinationStatus = { "Fully Vaccinated", "Partially Vaccinated", "Not Vaccinated" };

                string name = req.Query["name"];
                string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
                dynamic data = JsonConvert.DeserializeObject(requestBody);

                name = name ?? data?.name;

                int index = Array.FindIndex(ids, id => id.Equals(name, StringComparison.OrdinalIgnoreCase));
                string active = "ID not Found.";


                if (index != -1)
                {
                    active =
                        "\n-------------------------------------------------" +
                        "\nVaccination Information" +
                        "\n-------------------------------------------------" +
                        "\nID: " + ids[index] +
                        "\nName: " + names[index] +
                        "\nVaccination Centre: " + centres[index] +
                        "\nVaccine Name: " + vaccines[index] +
                        "\nVaccination Status: " + vaccinationStatus[index] +
                        "\n" + "-------------------------------------------------";


                }

                string responseMessage = string.IsNullOrEmpty(name)
                    ? $"{active}"
                    : $"{active}\n\nThis HTTP triggered function executed successfully.";

                return new OkObjectResult(responseMessage);
            }



        }
    }


