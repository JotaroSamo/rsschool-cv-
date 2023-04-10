<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
</head>
<body>
	<h1>Антон Самошук Юрьевич</h1>
	<h1><strong>Контакты для связи:</strong></h1>
	<p> телефон: +375 (29) 558-06-32, электронная почта: toni.samoshuk@gmail.com</p>
	<h1><strong>Краткая информация о себе:</strong></h1>
	<p> я целеустремленный и общительный начинающий разработчик .NET C# (Asp.core). Моей главной целью является непрерывное обучение и развитие своих навыков в сфере программирования. Я хорошо знаю русский язык (родной) и имею уровень А2+ по английскому языку.</p>
	<h1><strong>Навыки:</strong></h1>
	<p><ul>
  <li>Языки программирования: .Net C# (Asp.Core)</li>
  <li>Фреймворки: Entity Fraemwork.Core</li>
  <li>MSSQl</li>
  <li>Системы контроля версий: Git</li>
  <li>Инструменты разработки: Visual Studio Code, Visual Studio</li>
</ul></p>
<h1><strong>Примеры кода:</strong></h1>
	<p> примеры моего кода доступны на моей странице на GitHub: <a href="[https://github.com/JotaroSamo]">https://github.com/JotaroSamo</a>
		
<div><pre><code class="language-csharp">
using System.Security.Claims;
using TESTINGAPP.BusinessLogic.Interfaces;
using TESTINGAPP.Common.Dto;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Authorization;


namespace TESTINGAPP.Controllers
{
    public class AuthController : Controller
    {

        private readonly ILogger<AuthController> _logger;
        private readonly IUserService _userService;

        public AuthController(IUserService userService, ILogger<AuthController> logger)
        {
            _userService = userService;
            _logger = logger;
        }

        public IActionResult AuthPage()
        {
            _logger.LogInformation($"{DateTime.Now} Method AuthPage has been called.");
            return View();
        }
        public IActionResult RegPage()
        {
            _logger.LogInformation($"{DateTime.Now} Method AuthPage has been called.");
            return View();
        }
        [HttpPost]
        public async Task<IActionResult> RegPage(UserCreateDto model)
        {
            _logger.LogInformation($"{DateTime.Now} Method RegPage has been called with model {@model}.", model);
            

            if (await _userService.GetCheckAsync(_userService.Maping(model)) == null)
            {
                await _userService.CreateAsync(model);
                _logger.LogInformation("User with name {Name} and email {Email} has been created. {3}", model.Name, model.Email,DateTime.Now);

                return RedirectToAction("Index", "Home");
            }

            _logger.LogWarning("User with email {Email} already exists. {2}", model.Email, DateTime.Now);

            return View(model);
        }
        [HttpPost]
        public async Task<IActionResult> Auth(UserAuthDto userAuthDto)
        {
            var user = await _userService.GetAsync(userAuthDto);
            if (user == null)
            {
                _logger.LogWarning("Failed login attempt for user {userAuthDto.Email} at {DateTime.Now}", userAuthDto.Email, DateTime.Now);
                return RedirectToAction("RegPage");
            }

            var claims = new List<Claim>()
    {
                new Claim(ClaimTypes.NameIdentifier, user.Id.ToString()),
            new Claim(ClaimTypes.Name, user.Name),
        new Claim(ClaimTypes.Email, user.Email),
        new Claim(ClaimTypes.Role, user.Role)
    };

            if (user.Role == "Admin")
            {
                _logger.LogInformation("Admin with email {userAuthDto.Email} has logged in at {DateTime.Now}", userAuthDto.Email, DateTime.Now);
                var claimIdentity = new ClaimsIdentity(claims, CookieAuthenticationDefaults.AuthenticationScheme);
                await HttpContext.SignInAsync(CookieAuthenticationDefaults.AuthenticationScheme, new ClaimsPrincipal(claimIdentity));
                return RedirectToAction("Tools", "Admin");
            }

            _logger.LogInformation("User with email {userAuthDto.Email} has logged in at {DateTime.Now}", userAuthDto.Email, DateTime.Now);
            var userClaimsIdentity = new ClaimsIdentity(claims, CookieAuthenticationDefaults.AuthenticationScheme);
            await HttpContext.SignInAsync(CookieAuthenticationDefaults.AuthenticationScheme, new ClaimsPrincipal(userClaimsIdentity));
            return RedirectToAction("UserTools", "UserWork");
        }


    }
}
</code></pre></div>
</p>
<h1><strong>Опыт работы:</strong></h1>
	<p> пока что у меня нет опыта работы, но я учусь и активно работаю над собственными проектами. В ходе учебы я успешно реализовал несколько проектов, используя .NET C# (Asp.core), Git и Visual Studio.</p>
	<h1><strong>Образование:</strong></h1>
	<p> я учусь в Полесском государственном университете до 2024 года. .</p>
	<h1><strong>Английский язык:</strong></h1>
	<p> мой уровень английского языка - А2+.</p>
</body>
</html>
