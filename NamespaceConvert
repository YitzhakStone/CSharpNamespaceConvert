using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Collections;

namespace ENTIDADES_BLL
{

    // EM TESTE !!!, não usar - (Yitzhak)

    public static class CustomCast
    {

        public static T CustomCast<T>(this object myobj) where T : class
        {

            //throw new NotImplementedException("Ainda em testes, não usar!!! - Yitzhak");
            return (T)myobj.CustomCast(typeof(T));

        }

        private static object CustomCast(this object myobj, Type newType)
        {

            //throw new NotImplementedException("Ainda em testes, não usar!!! - Yitzhak");

            Type objectType = myobj.GetType();
            Type target = newType;
            var x = Activator.CreateInstance(target, false);
            var z = from source in objectType.GetMembers().ToList()
                    where source.MemberType == System.Reflection.MemberTypes.Property || source.MemberType == System.Reflection.MemberTypes.Field
                    select source;
            var d = from source in target.GetMembers().ToList()
                    where source.MemberType == System.Reflection.MemberTypes.Property || source.MemberType == System.Reflection.MemberTypes.Field
                    select source;
            List<System.Reflection.MemberInfo> members = d.Where(memberInfo => d.Select(c => c.Name)
               .ToList().Contains(memberInfo.Name)).ToList();
            System.Reflection.PropertyInfo propertyInfo;
            object value;
            foreach (var memberInfo in members)
            {
                if (myobj.GetType().GetProperty(memberInfo.Name) != null)
                {
                    propertyInfo = newType.GetProperty(memberInfo.Name);
                    value = myobj.GetType().GetProperty(memberInfo.Name).GetValue(myobj, null);
                    if (value != null)
                    {
                        if (value.GetType().IsArray)
                        {
                            Array x2 = Array.CreateInstance(propertyInfo.PropertyType.GetElementType(), (value as Array).Length);
                            IEnumerable enumerable = value as IEnumerable;
                            if (enumerable != null)
                            {
                                int i = 0;
                                foreach (object element in enumerable)
                                {
                                    object newElement = element.CustomCast(propertyInfo.PropertyType.GetElementType());
                                    x2.SetValue(newElement, i++);
                                }
                            }
                            value = x2;
                        }
                        else if (value.GetType().IsGenericType)
                        {
                            // implementar aqui listas, pilhas, filas... tipos genéricos: Classe<T>
                        }
                        else if (propertyInfo.PropertyType != typeof(string) && !value.GetType().IsValueType)
                        {
                            value = value.CustomCast(propertyInfo.PropertyType);
                        }
                    }
                    propertyInfo.SetValue(x, value, null);
                }
                else if (myobj.GetType().GetField(memberInfo.Name) != null)
                {
                    System.Reflection.MemberInfo[] m = newType.GetMember(memberInfo.Name);
                    value = myobj.GetType().GetField(memberInfo.Name).GetValue(myobj);
                    x.GetType().GetField(memberInfo.Name).SetValue(x, value);
                }
            } // fim: foreach memberInfo in members

            return Convert.ChangeType(x, newType);

        }

    }

}
