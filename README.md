# assignments-streamss

import countries.Country;
import countries.CountryRepository;
import countries.Region;

import java.math.BigDecimal;
import java.util.*;

class Scratch {
    public static void main(String[] args) {
        var countries = new CountryRepository().getAll();


   var SumEU =countries.stream()
                .filter(country -> country.getRegion() == Region.EUROPE)
                .mapToLong(Country::getPopulation)
                .sum();

    System.out.println(SumEU);

//assignment 13
        countries.stream()
                .filter(country -> country.getRegion() == Region.EUROPE)
                .mapToLong(Country::getPopulation)
                .boxed()
                .sorted(Comparator.reverseOrder())
                .forEach(System.out::println);

//assignment 14
        countries.stream()
                .filter(country -> country.getRegion() == Region.EUROPE)
                .sorted(Comparator.comparingLong(Country::getPopulation))
                .forEach(country -> System.out.printf("%s : %d\n",
                        country.getName(), country.getPopulation()));

        List<Country> toSort = new ArrayList<>();
        for (Country country : countries) {
            if (country.getRegion() == Region.EUROPE) {
                toSort.add(country);
            }
        }
        toSort.sort(Comparator.comparingLong(Country::getPopulation));
        for (Country country : toSort) {
            System.out.printf("%s : %d\n",
                    country.getName(), country.getPopulation());
        }

        //assignment 15
        countries.stream()
                .filter(country -> country.getRegion() == Region.EUROPE)
                .max(Comparator.comparingLong(Country::getPopulation))
                .ifPresent(System.out::println);

        //assignment 17
        countries.stream()
                .map(Country::getName)
                .limit(5)
                .forEach(System.out::println);

        //assihnment 18
       var exists =countries.stream()
                .anyMatch(country -> country.getPopulation()==0);

       System.out.println(exists);

       //assignment 19
        var tmexist =countries.stream()
                .allMatch(country -> country.getTimezones().size() > 0);
        System.out.println(tmexist);

        //assignment 20
        countries.stream()
                .filter(country -> country.getName().charAt(0) == 'H')
                .findFirst()
                .ifPresent(System.out::println);

        //assignment 21
        var TZ = countries.stream()
                .flatMap(country -> country.getTimezones().stream())
                .distinct()
                .count()
                //.forEach(System.out::println)
                ;
        System.out.println(TZ);

        //assignment 24
        countries.stream()
                .map(country -> country.getName())
                .mapToInt(String::length)
                .max()
                .ifPresent(System.out::println);
        //longest country name, the actual name
        countries.stream()
                .map(country -> country.getName())
                .max(Comparator.comparingInt(String::length))
                .ifPresent(System.out::println);

        //countries 2 assignments
        //assignment 1

        var exists2 = countries.stream()
                .map(Country::getName)
                .map(String::toLowerCase)
                .anyMatch(s -> s.contains("island"));
        System.out.println(exists2);


        //assignment 2
        countries.stream()
                .map(Country::getName)
                //.map(String::toLowerCase)
                .filter(s -> s.toLowerCase().contains("island"))
                .findFirst()
                .ifPresent(System.out::println);

        //assignment 3
        countries.stream()
                .map(Country::getName)
                .filter(s -> {
                    s = s.toLowerCase();
                    return s.charAt(0) == s.charAt(s.length() -1);
                })
                .forEach(System.out::println);

        //assignment 7
        countries.stream()
                .sorted(Comparator.comparingInt(country -> country.getTimezones().size()))
                .map(country -> country.getName())
                .forEach(System.out::println);

        //assignment 8
        countries.stream()
                .sorted(Comparator.comparingInt(country -> country.getTimezones().size()))

                .forEach(country -> System.out.printf("%s : %d\n",country.getName(), country.getTimezones().size()));

        //assignment 9
        var EsName = countries.stream()
                .filter(country -> ! country.getTranslations().containsKey("es"))
                .count();
        System.out.println(EsName);

        //assignment 10
        countries.stream()
                .filter(country -> country.getArea() == null)
                .map(country -> country.getName())
                .forEach(System.out::println);

        //assignment 11
        countries.stream()
                .flatMap(country -> country.getTranslations().keySet().stream())
                .sorted()
                .distinct()
                .forEach(System.out::println);

        //countries 3 assignments
        //assignment 1
        countries.stream()
                .filter(country -> country.getArea() != null)
                .max(Comparator.comparing(Country::getArea))
                .ifPresent(System.out::println);


       var TotalArea =  countries.stream()
                .map(country -> country.getArea())
                .filter(Objects::nonNull)
                .reduce(BigDecimal.ZERO, BigDecimal::add);
        System.out.println(TotalArea);

        //assignment 6

        var CountryNameMap =  countries.stream()
                .collect(HashMap<String, String>::new, (map, country) ->map.put(country.getCode(),country.getName()), HashMap::putAll);
        System.out.println(CountryNameMap);
        System.out.println(CountryNameMap.get("HU"));






